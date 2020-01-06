---
title: ASP.NET Core 'de model bağlama
author: rick-anderson
description: ASP.NET Core model bağlamasının nasıl çalıştığını ve davranışını nasıl özelleştireceğinizi öğrenin.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 12/18/2019
uid: mvc/models/model-binding
ms.openlocfilehash: d36e42ef2517068ade3f874dc62cc7587ee3ca98
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355668"
---
# <a name="model-binding-in-aspnet-core"></a>ASP.NET Core 'de model bağlama

::: moniker range=">= aspnetcore-3.0"

Bu makalede, model bağlamanın ne olduğu, nasıl çalıştığı ve davranışını nasıl özelleştireceğiniz açıklanmaktadır.

[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Model bağlama nedir?

Denetleyiciler ve Razor sayfaları, HTTP isteklerinden gelen verilerle çalışır. Örneğin, rota verileri bir kayıt anahtarı sağlayabilir ve postalanan form alanları, modelin özelliklerine ilişkin değerler sağlayabilir. Bu değerlerin her birini almak ve bunları dizelerden .NET türlerine dönüştürmek için kod yazma sıkıcı ve hata durumunda olabilir. Model bağlama bu işlemi otomatikleştirir. Model bağlama sistemi:

* Veri yolu, form alanları ve sorgu dizeleri gibi çeşitli kaynaklardan veri alır.
* Yöntem parametrelerinde ve genel özelliklerde bulunan denetleyicilere ve Razor sayfalarına verileri sağlar.
* Dize verilerini .NET türlerine dönüştürür.
* Karmaşık türlerin özelliklerini güncelleştirir.

## <a name="example"></a>Örnek

Aşağıdaki eylem yöntemine sahip olduğunuzu varsayalım:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

Ve uygulama şu URL ile bir istek alıyor:

```
http://contoso.com/api/pets/2?DogsOnly=true
```

Model bağlama, yönlendirme sistemi eylem yöntemini seçtikten sonra aşağıdaki adımlardan geçer:

* `id`adlı bir tamsayı olan `GetByID`ilk parametresini bulur.
* HTTP isteğindeki kullanılabilir kaynakları arar ve yönlendirme verilerinde `id` = "2" bulur.
* "2" dizesini tamsayı 2 ' ye dönüştürür.
* `dogsOnly`adlı bir Boole değeri olan `GetByID`sonraki parametresini bulur.
* Kaynakları arar ve sorgu dizesinde "DogsOnly = true" bulur. Ad eşleştirme, büyük/küçük harfe duyarlı değildir.
* "True" dizesini Boole `true`dönüştürür.

Daha sonra Framework, `id` parametresi için 2 ' ye geçerek ve `dogsOnly` parametresi için `true` `GetById` yöntemini çağırır.

Önceki örnekte, model bağlama hedefleri basit türler olan yöntem parametreleridir. Hedefler, karmaşık bir türün özellikleri de olabilir. Her bir özellik başarıyla bağlandıktan sonra, bu özellik için [model doğrulaması](xref:mvc/models/validation) oluşur. Hangi verilerin modele bağladığına ve tüm bağlama veya doğrulama hatalarıyla ilgili kayıt, [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) veya [Pagemodel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)içinde depolanır. Bu işlemin başarılı olup olmadığını öğrenmek için uygulama [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) bayrağını denetler.

## <a name="targets"></a>Hedefler

Model bağlama, aşağıdaki tür hedeflerin değerlerini bulmayı dener:

* Bir isteğin yönlendirildiği denetleyici eylemi yönteminin parametreleri.
* Bir isteğin yönlendirildiği Razor Pages işleyicisi yönteminin parametreleri. 
* Bir denetleyicinin veya `PageModel` sınıfının öznitelikler tarafından belirtilmişse ortak özellikleri.

### <a name="bindproperty-attribute"></a>[BindProperty] özniteliği

Bir denetleyicinin veya `PageModel` sınıfın ortak özelliğine, model bağlamasının bu özelliği hedeflemesini sağlamak için uygulanabilir:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a>[BindProperties] özniteliği

ASP.NET Core 2,1 ve üzeri sürümlerde kullanılabilir.  Model bağlamaya, sınıfın tüm ortak özelliklerini hedeflemesini bildirmek için bir denetleyiciye veya `PageModel` sınıfa uygulanabilir:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>HTTP GET istekleri için model bağlama

Varsayılan olarak, Özellikler HTTP GET istekleri için bağlantılı değildir. Genellikle, bir GET isteği için tüm ihtiyacınız olan bir kayıt KIMLIĞI parametresidir. Kayıt KIMLIĞI, veritabanındaki öğeyi aramak için kullanılır. Bu nedenle, modelin bir örneğini tutan bir özelliği bağlamaya gerek yoktur. GET isteklerinden alınan özelliklerin verilerine bağlanmasını istediğiniz senaryolarda `SupportsGet` özelliğini `true`olarak ayarlayın:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Kaynaklar

Varsayılan olarak, model bağlama, bir HTTP isteğindeki aşağıdaki kaynaklardan gelen anahtar-değer çiftleri biçimindeki verileri alır:

1. Form alanları
1. İstek gövdesi ( [[ApiController] özniteliğine sahip denetleyiciler](xref:web-api/index#binding-source-parameter-inference)için.)
1. Veri yönlendirme
1. Sorgu dizesi parametreleri
1. Karşıya yüklenen dosyalar

Her hedef parametresi veya özelliği için kaynaklar, önceki listede belirtilen sırada taranır. Birkaç özel durum vardır:

* Rota verileri ve sorgu dizesi değerleri yalnızca basit türler için kullanılır.
* Karşıya yüklenen dosyalar yalnızca `IFormFile` veya `IEnumerable<IFormFile>`uygulayan hedef türlere bağlanır.

Varsayılan kaynak doğru değilse, kaynağı belirtmek için aşağıdaki özniteliklerden birini kullanın:

* [`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -sorgu dizesinden değerleri alır. 
* [`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -rota verilerinden değerleri alır.
* [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -postalanan Form alanlarındaki değerleri alır.
* [`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -istek gövdesinden değerleri alır.
* [`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -http başlıklarındaki değerleri alır.

Bu öznitelikler:

* Model özelliklerine tek tek eklenir (model sınıfına değil), aşağıdaki örnekte olduğu gibi:

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* İsteğe bağlı olarak oluşturucuda bir model adı değeri kabul edin. Bu seçenek, özellik adının istekteki değerle eşleşmemesi durumunda sağlanır. Örneğin, istekteki değer aşağıdaki örnekte olduğu gibi adında bir tire olan bir üstbilgi olabilir:

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>[FromBody] özniteliği

Bir HTTP isteğinin gövdesinden özelliklerini doldurmak için `[FromBody]` özniteliğini bir parametreye uygulayın. ASP.NET Core çalışma zamanı, gövdeyi bir giriş biçimlendirici 'ya okumaktan sorumlu olarak temsil eder. Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.

Karmaşık bir tür parametresine `[FromBody]` uygulandığında, özelliklerine uygulanan herhangi bir bağlama kaynak özniteliği yok sayılır. Örneğin, aşağıdaki `Create` eylemi `pet` parametresinin gövdeden doldurulduğunu belirtir:

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

`Pet` sınıfı, `Breed` özelliğinin bir sorgu dizesi parametresinden doldurulduğunu belirtir:

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

Yukarıdaki örnekte:

* `[FromQuery]` özniteliği yok sayılır.
* `Breed` özelliği bir sorgu dizesi parametresinden doldurulmamış. 

Giriş biçimleri yalnızca gövdeyi okur ve bağlama kaynak özniteliklerini anlamayın. Gövdede uygun bir değer bulunursa, bu değer `Breed` özelliğini doldurmak için kullanılır.

Eylem yöntemi başına birden fazla parametreye `[FromBody]` uygulamayın. İstek akışı bir giriş biçimlendirici tarafından okunduktan sonra, diğer `[FromBody]` parametrelerini bağlamak için artık bir daha okunamaz.

### <a name="additional-sources"></a>Ek kaynaklar

Kaynak verileri, model bağlama sistemine *değer sağlayıcılara*göre sağlanır. Diğer kaynaklardan model bağlamaya yönelik verileri alan özel değer sağlayıcıları yazabilir ve kaydedebilirsiniz. Örneğin, tanımlama bilgileri veya oturum durumu verilerini isteyebilirsiniz. Yeni bir kaynaktan veri almak için:

* `IValueProvider`uygulayan bir sınıf oluşturun.
* `IValueProviderFactory`uygulayan bir sınıf oluşturun.
* Factory sınıfını `Startup.ConfigureServices`kaydedin.

Örnek uygulama, tanımlama bilgilerinden değerler alan bir [değer sağlayıcısı](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) ve [Factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) örneği içerir. `Startup.ConfigureServices`kayıt kodu aşağıda verilmiştir:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4)]

Gösterilen kod, tüm yerleşik değer sağlayıcılarından sonra özel değer sağlayıcısını koyar.  Listenin ilk olması için, `Add`yerine `Insert(0, new CookieValueProviderFactory())` çağırın.

## <a name="no-source-for-a-model-property"></a>Model özelliği için kaynak yok

Varsayılan olarak, model özelliği için bir değer bulunmazsa model durumu hatası oluşturulmaz. Özelliği null veya varsayılan bir değer olarak ayarlanır:

* Null yapılabilir basit türler `null`olarak ayarlanır.
* Null yapılamayan değer türleri `default(T)`olarak ayarlanır. Örneğin, `int id` parametresi 0 olarak ayarlanır.
* Karmaşık türler için model bağlama, özellikleri ayarlamadan varsayılan oluşturucuyu kullanarak bir örnek oluşturur.
* Diziler, `byte[]` dizilerinin `null`olarak ayarlandığı durumlar dışında `Array.Empty<T>()`olarak ayarlanır.

Model özelliği için form alanlarında hiçbir şey bulunamadığında model durumunun geçersiz kılınmalıdır, [`[BindRequired]`](#bindrequired-attribute) özniteliğini kullanın.

Bu `[BindRequired]` davranışının, bir istek gövdesinde JSON veya XML verilerine değil, postalanan form verilerinden model bağlama için geçerli olduğunu unutmayın. İstek gövdesi verileri, [giriş formatlayıcıları](#input-formatters)tarafından işlenir.

## <a name="type-conversion-errors"></a>Tür dönüştürme hataları

Bir kaynak bulunursa ancak hedef türe dönüştürülemiyorsa, model durumu geçersiz olarak işaretlenir. Hedef parametresi veya özelliği, önceki bölümde belirtildiği gibi null veya varsayılan değer olarak ayarlanır.

`[ApiController]` özniteliğine sahip bir API denetleyicisinde, geçersiz model durumu otomatik HTTP 400 yanıtına neden olur.

Razor sayfasında, sayfayı bir hata iletisiyle yeniden görüntüleyin:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

İstemci tarafı doğrulama, aksi durumda Razor Pages bir forma gönderilemeyen hatalı verileri yakalar. Bu doğrulama, önceki vurgulanmış kodu tetiklemeyi zorlaştırır. Örnek uygulama, teslim **tarihi** alanına hatalı veri yerleştiren ve formu Gönderen **Geçersiz tarih içeren bir Gönder** düğmesi içerir. Bu düğme, veri dönüştürme hataları oluştuğunda sayfanın yeniden görüntülenmesine yönelik kodun nasıl çalıştığını gösterir.

Sayfa önceki kodla yeniden görüntülendiğinde, form alanında geçersiz giriş gösterilmez. Bunun nedeni model özelliğinin null ya da varsayılan bir değer olarak ayarlanmış olmasından kaynaklanır. Geçersiz giriş bir hata iletisinde görüntülenir. Ancak form alanındaki hatalı verileri yeniden görüntülemek istiyorsanız, model özelliğini bir dize haline getirmeyi ve veri dönüştürmeyi el ile gerçekleştirmeyi düşünün.

Tür dönüştürme hatalarının model durumu hatalarına neden olmasını istemiyorsanız aynı strateji önerilir. Bu durumda model özelliğini bir dize yapın.

## <a name="simple-types"></a>Basit türler

Model cildin kaynak dizeleri dönüştürebileceğiniz basit türler aşağıdakileri içerir:

* [Boole değeri](xref:System.ComponentModel.BooleanConverter)
* [Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)
* [Char](xref:System.ComponentModel.CharConverter)
* [Hem](xref:System.ComponentModel.DateTimeConverter)
* [Türünde](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Kategori](xref:System.ComponentModel.DecimalConverter)
* [Çift](xref:System.ComponentModel.DoubleConverter)
* [Yardımının](xref:System.ComponentModel.EnumConverter)
* ['İni](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Sunuculu](xref:System.ComponentModel.SingleConverter)
* [TimeSpan](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [Uri](xref:System.UriTypeConverter)
* [Sürüm](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Karmaşık türler

Karmaşık bir türün bağlanması için ortak bir varsayılan Oluşturucusu ve ortak yazılabilir özellikleri olmalıdır. Model bağlama gerçekleştiğinde, sınıf ortak varsayılan Oluşturucu kullanılarak oluşturulur. 

Karmaşık türün her özelliği için model bağlama, ad modeli ön eki için kaynakları arar *. property_name*. Hiçbir şey bulunamazsa, ön ek olmadan yalnızca *property_name* arar.

Bir parametreye bağlama için, önek parametre adıdır. `PageModel` public özelliğine bağlama için, önek ortak özellik adıdır. Bazı özniteliklerin, parametre veya özellik adının varsayılan kullanımını geçersiz kılabilmenizi sağlayan `Prefix` bir özelliği vardır.

Örneğin, karmaşık türün aşağıdaki `Instructor` sınıfı olduğunu varsayalım:

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Önek = parametre adı

Bağlanacak model `instructorToUpdate`adlı bir parametredir:

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

Model bağlama, anahtar `instructorToUpdate.ID`kaynaklara bakarak başlar. Bu bulunamazsa, öneki olmayan `ID` arar.

### <a name="prefix--property-name"></a>Önek = Özellik adı

Bağlanacak model, denetleyicinin veya `PageModel` sınıfının `Instructor` adlı bir özelliktir:

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar. Bu bulunamazsa, öneki olmayan `ID` arar.

### <a name="custom-prefix"></a>Özel ön ek

Bağlanacak model `instructorToUpdate` adlı bir parametredir ve `Bind` özniteliği önek olarak `Instructor` belirtir:

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar. Bu bulunamazsa, öneki olmayan `ID` arar.

### <a name="attributes-for-complex-type-targets"></a>Karmaşık tür hedefleri için öznitelikler

Karmaşık türlerin model bağlamasını denetlemek için birkaç yerleşik öznitelik mevcuttur:

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> Bu öznitelikler, gönderilen form verileri değer kaynağı olduğunda model bağlamayı etkiler. Bunlar, gönderilen JSON ve XML istek gövdelerini işleyen giriş formatlayıcıları 'nı etkilemez. Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.
>
> Ayrıca bkz. [model doğrulamasında](xref:mvc/models/validation#required-attribute)`[Required]` özniteliği tartışması.

### <a name="bindrequired-attribute"></a>[BindRequired] özniteliği

, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir. Modelin özelliği için bağlama gerçekleşmemişse model bağlamasının model durumu hatası eklemesine neden olur. Örnek buradadır:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>[Bindhiç] özniteliği

, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir. Model bağlamasının model özelliğini değiştirmesini engeller. Örnek buradadır:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>[Bind] özniteliği

, Bir sınıfa veya yöntem parametresine uygulanabilir. Model bağlamasındaki bir modelin hangi özelliklerinin dahil edileceğini belirtir.

Aşağıdaki örnekte, herhangi bir işleyici veya eylem yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

Aşağıdaki örnekte, `OnPost` yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

`[Bind]` özniteliği, *oluşturma* senaryolarında fazla nakline karşı korumak için kullanılabilir. Dışlanan Özellikler null ya da boş değer olarak ayarlandığı için, düzenleme senaryolarında iyi çalışmaz. Fazla nakline karşı savunma için, `[Bind]` özniteliği yerine, görüntüleme modelleri önerilir. Daha fazla bilgi için bkz. fazla [nakil hakkında güvenlik NOI](xref:data/ef-mvc/crud#security-note-about-overposting).

## <a name="collections"></a>Koleksiyonlar

Basit türlerin koleksiyonları olan hedefler için model bağlama, *parameter_name* veya *property_name*ile eşleşmeleri arar. Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar. Örneğin:

* Bağlanacak parametrenin `selectedCourses`adlı bir dizi olduğunu varsayalım:

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

* Aşağıdaki biçim yalnızca form verilerinde desteklenir:

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* Önceki örnek biçimlerinin hepsi için model bağlama iki öğe dizisini `selectedCourses` parametresine geçirir:

  * Selectedkurslar [0] = 1050
  * Selectedkurslar [1] = 2000

  Alt simge numaralarını kullanan veri biçimleri (... [0]... [1]...) sıfırdan başlayarak sıralı olarak numaralandırıldıklarından emin olmalıdır. Alt simge numaralandırmasında boşluk varsa, boşluklardan sonraki tüm öğeler yoksayılır. Örneğin, alt simgeler 0 ve 1 yerine 0 ve 2 ise ikinci öğe yok sayılır.

## <a name="dictionaries"></a>Sözlükler

`Dictionary` hedefler için, model bağlama *parameter_name* veya *property_name*eşleşmelerini arar. Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar. Örneğin:

* Hedef parametrenin `selectedCourses`adlı bir `Dictionary<int, string>` olduğunu varsayalım:

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Postalanan form veya sorgu dizesi verileri aşağıdaki örneklerden birine benzeyebilir:

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

* Önceki örnek biçimlerinin hepsi için model bağlama iki öğenin sözlüğünü `selectedCourses` parametresine geçirir:

  * Selectedkurslar ["1050"] = "Chemistry"
  * Selectedkurslar ["2000"] = "Ekonomiks"

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a>Model bağlama yolu verileri ve sorgu dizeleri için Genelleştirme davranışı

ASP.NET Core yol değeri sağlayıcısı ve sorgu dizesi değer sağlayıcısı:

* Değerleri sabit kültür olarak değerlendirin.
* URL 'Lerin kültür sabiti olmasını bekler.

Buna karşılık, form verilerinden gelen değerler kültüre duyarlı bir dönüştürmeye gider. Bu, URL 'Lerin yerel ayarlarda paylaşılabilir olması için tasarımdır.

ASP.NET Core yol değeri sağlayıcısını ve sorgu dizesi değeri sağlayıcısını, kültüre duyarlı bir dönüşüme dönüştürmek için:

* <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory> 'den devralma
* [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) veya [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs) adresinden kodu kopyalayın
* Değer sağlayıcısı oluşturucusuna geçirilen [kültür değerini](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture) ile değiştirin
* MVC seçeneklerinde varsayılan değer sağlayıcı fabrikasını yeni bir değerle değiştirin:

[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a>Özel veri türleri

Model bağlamanın işleyebileceği bazı özel veri türleri vardır.

### <a name="iformfile-and-iformfilecollection"></a>Iformfile ve ıformfilecollection

HTTP isteğine eklenen karşıya yüklenen dosya.  Ayrıca, birden çok dosya için `IEnumerable<IFormFile>` desteklenir.

### <a name="cancellationtoken"></a>CancellationToken

Zaman uyumsuz denetleyicilerde etkinliği iptal etmek için kullanılır.

### <a name="formcollection"></a>Form koleksiyonu

Postalanan form verilerinden tüm değerleri almak için kullanılır.

## <a name="input-formatters"></a>Giriş biçimleri

İstek gövdesindeki veriler JSON, XML veya başka bir biçimde olabilir. Model bağlama, bu verileri ayrıştırmak için belirli bir içerik türünü işlemek üzere yapılandırılmış bir *giriş biçimlendiricisi* kullanır. Varsayılan olarak, ASP.NET Core JSON verilerini işlemek için JSON tabanlı giriş formatlayıcıları içerir. Diğer içerik türleri için başka biçim ekleyebilirsiniz.

ASP.NET Core, [tüketen](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) özniteliğine bağlı olarak giriş formatlayıcıları seçer. Hiçbir öznitelik yoksa, [Content-Type üst bilgisini](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)kullanır.

Yerleşik XML girişi formatlayıcıları 'nı kullanmak için:

* `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketini yükler.

* `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> veya <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>çağırın.

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=10)]

* `Consumes` özniteliğini, istek gövdesinde XML beklemeniz gereken denetleyici sınıflarına veya eylem yöntemlerine uygulayın.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Daha fazla bilgi için bkz. [XML serileştirme tanıtımı](/dotnet/standard/serialization/introducing-xml-serialization).

### <a name="customize-model-binding-with-input-formatters"></a>Giriş formatlayıcıları ile model bağlamayı özelleştirme

Giriş biçimlendiricisi, istek gövdesinden veri okumak için tam sorumluluğa sahiptir. Bu işlemi özelleştirmek için, giriş biçimlendirici tarafından kullanılan API 'Leri yapılandırın. Bu bölümde, `ObjectId`adlı özel bir türü anlamak için `System.Text.Json`tabanlı giriş biçimlendiricinin nasıl özelleştirileceği açıklanmaktadır. 

`Id`adlı özel bir `ObjectId` özelliği içeren aşağıdaki modeli göz önünde bulundurun:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ModelWithObjectId.cs?name=snippet_Class&highlight=3)]

`System.Text.Json`kullanırken model bağlama işlemini özelleştirmek için, <xref:System.Text.Json.Serialization.JsonConverter%601>türetilmiş bir sınıf oluşturun:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/JsonConverters/ObjectIdConverter.cs?name=snippet_Class)]

Özel bir dönüştürücü kullanmak için <xref:System.Text.Json.Serialization.JsonConverterAttribute> özniteliğini türe uygulayın. Aşağıdaki örnekte, `ObjectId` türü özel dönüştürücü olarak `ObjectIdConverter` ile yapılandırılır:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ObjectId.cs?name=snippet_Class&highlight=1)]

Daha fazla bilgi için bkz. [özel dönüştürücüler yazma](/dotnet/standard/serialization/system-text-json-converters-how-to).

## <a name="exclude-specified-types-from-model-binding"></a>Belirtilen türleri model bağlamalarından Dışla

Model bağlama ve doğrulama sistemlerinin davranışı [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata)tarafından yönlendiriliyor. [Mvcoptions. Modelmetadatadetails sağlayıcılarına](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)bir ayrıntı sağlayıcısı ekleyerek `ModelMetadata` özelleştirebilirsiniz. Yerleşik Ayrıntılar sağlayıcıları, belirtilen türler için model bağlamayı veya doğrulamayı devre dışı bırakmak üzere kullanılabilir.

Belirtilen türdeki tüm modellerdeki model bağlamayı devre dışı bırakmak için `Startup.ConfigureServices`bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> ekleyin. Örneğin, `System.Version`türündeki tüm modeller üzerinde model bağlamayı devre dışı bırakmak için:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=5-6)]

Belirtilen türdeki özelliklerde doğrulamayı devre dışı bırakmak için `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> ekleyin. Örneğin, `System.Guid`türündeki özelliklerde doğrulamayı devre dışı bırakmak için:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=7-8)]

## <a name="custom-model-binders"></a>Özel model ciltleri

Özel bir model cildi yazarak ve belirli bir hedef için seçmek üzere `[ModelBinder]` özniteliğini kullanarak model bağlamayı genişletebilirsiniz. [Özel model bağlama](xref:mvc/advanced/custom-model-binding)hakkında daha fazla bilgi edinin.

## <a name="manual-model-binding"></a>El ile model bağlama 

Model bağlama, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> yöntemi kullanılarak el ile çağrılabilir. Yöntemi hem `ControllerBase` hem de `PageModel` sınıflarında tanımlanmıştır. Yöntem aşırı yüklemeleri, kullanılacak öneki ve değer sağlayıcısını belirtmenizi sağlar. Model bağlama başarısız olursa Yöntem `false` döndürür. Örnek buradadır:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>, form gövdesinden, sorgu dizesinden ve veri rota verilerinden veri almak için değer sağlayıcılarını kullanır. `TryUpdateModelAsync` genellikle: 

* Daha fazla nakletmeyi engellemek için denetleyiciler ve görünümler kullanan Razor Pages ve MVC uygulamalarıyla birlikte kullanılır.
* Form verilerinden, sorgu dizelerinden ve veri rotalarından tüketilmediği müddetçe bir Web API 'SI ile kullanılmaz. JSON kullanan Web API uç noktaları, istek gövdesini bir nesne olarak seri durumdan çıkarmak için [giriş formatlarını](#input-formatters) kullanır.

Daha fazla bilgi için bkz. [Tryupdatemodelasync](xref:data/ef-rp/crud#TryUpdateModelAsync).

## <a name="fromservices-attribute"></a>[FromServices] özniteliği

Bu özniteliğin adı, bir veri kaynağı belirten model bağlama özniteliklerinin düzeniyle uyar. Ancak bir değer sağlayıcısından veri bağlama hakkında bilgi yoktur. [Bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısından bir türün bir örneğini alır. Amacı, yalnızca belirli bir yöntem çağrılırsa bir hizmete ihtiyacınız olduğunda Oluşturucu ekleme için bir alternatif sağlamaktır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

Bu makalede, model bağlamanın ne olduğu, nasıl çalıştığı ve davranışını nasıl özelleştireceğiniz açıklanmaktadır.

[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Model bağlama nedir?

Denetleyiciler ve Razor sayfaları, HTTP isteklerinden gelen verilerle çalışır. Örneğin, rota verileri bir kayıt anahtarı sağlayabilir ve postalanan form alanları, modelin özelliklerine ilişkin değerler sağlayabilir. Bu değerlerin her birini almak ve bunları dizelerden .NET türlerine dönüştürmek için kod yazma sıkıcı ve hata durumunda olabilir. Model bağlama bu işlemi otomatikleştirir. Model bağlama sistemi:

* Veri yolu, form alanları ve sorgu dizeleri gibi çeşitli kaynaklardan veri alır.
* Yöntem parametrelerinde ve genel özelliklerde bulunan denetleyicilere ve Razor sayfalarına verileri sağlar.
* Dize verilerini .NET türlerine dönüştürür.
* Karmaşık türlerin özelliklerini güncelleştirir.

## <a name="example"></a>Örnek

Aşağıdaki eylem yöntemine sahip olduğunuzu varsayalım:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

Ve uygulama şu URL ile bir istek alıyor:

```
http://contoso.com/api/pets/2?DogsOnly=true
```

Model bağlama, yönlendirme sistemi eylem yöntemini seçtikten sonra aşağıdaki adımlardan geçer:

* `id`adlı bir tamsayı olan `GetByID`ilk parametresini bulur.
* HTTP isteğindeki kullanılabilir kaynakları arar ve yönlendirme verilerinde `id` = "2" bulur.
* "2" dizesini tamsayı 2 ' ye dönüştürür.
* `dogsOnly`adlı bir Boole değeri olan `GetByID`sonraki parametresini bulur.
* Kaynakları arar ve sorgu dizesinde "DogsOnly = true" bulur. Ad eşleştirme, büyük/küçük harfe duyarlı değildir.
* "True" dizesini Boole `true`dönüştürür.

Daha sonra Framework, `id` parametresi için 2 ' ye geçerek ve `dogsOnly` parametresi için `true` `GetById` yöntemini çağırır.

Önceki örnekte, model bağlama hedefleri basit türler olan yöntem parametreleridir. Hedefler, karmaşık bir türün özellikleri de olabilir. Her bir özellik başarıyla bağlandıktan sonra, bu özellik için [model doğrulaması](xref:mvc/models/validation) oluşur. Hangi verilerin modele bağladığına ve tüm bağlama veya doğrulama hatalarıyla ilgili kayıt, [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) veya [Pagemodel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState)içinde depolanır. Bu işlemin başarılı olup olmadığını öğrenmek için uygulama [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) bayrağını denetler.

## <a name="targets"></a>Hedefler

Model bağlama, aşağıdaki tür hedeflerin değerlerini bulmayı dener:

* Bir isteğin yönlendirildiği denetleyici eylemi yönteminin parametreleri.
* Bir isteğin yönlendirildiği Razor Pages işleyicisi yönteminin parametreleri. 
* Bir denetleyicinin veya `PageModel` sınıfının öznitelikler tarafından belirtilmişse ortak özellikleri.

### <a name="bindproperty-attribute"></a>[BindProperty] özniteliği

Bir denetleyicinin veya `PageModel` sınıfın ortak özelliğine, model bağlamasının bu özelliği hedeflemesini sağlamak için uygulanabilir:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a>[BindProperties] özniteliği

ASP.NET Core 2,1 ve üzeri sürümlerde kullanılabilir.  Model bağlamaya, sınıfın tüm ortak özelliklerini hedeflemesini bildirmek için bir denetleyiciye veya `PageModel` sınıfa uygulanabilir:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>HTTP GET istekleri için model bağlama

Varsayılan olarak, Özellikler HTTP GET istekleri için bağlantılı değildir. Genellikle, bir GET isteği için tüm ihtiyacınız olan bir kayıt KIMLIĞI parametresidir. Kayıt KIMLIĞI, veritabanındaki öğeyi aramak için kullanılır. Bu nedenle, modelin bir örneğini tutan bir özelliği bağlamaya gerek yoktur. GET isteklerinden alınan özelliklerin verilerine bağlanmasını istediğiniz senaryolarda `SupportsGet` özelliğini `true`olarak ayarlayın:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Kaynaklar

Varsayılan olarak, model bağlama, bir HTTP isteğindeki aşağıdaki kaynaklardan gelen anahtar-değer çiftleri biçimindeki verileri alır:

1. Form alanları
1. İstek gövdesi ( [[ApiController] özniteliğine sahip denetleyiciler](xref:web-api/index#binding-source-parameter-inference)için.)
1. Veri yönlendirme
1. Sorgu dizesi parametreleri
1. Karşıya yüklenen dosyalar

Her hedef parametresi veya özelliği için kaynaklar, önceki listede belirtilen sırada taranır. Birkaç özel durum vardır:

* Rota verileri ve sorgu dizesi değerleri yalnızca basit türler için kullanılır.
* Karşıya yüklenen dosyalar yalnızca `IFormFile` veya `IEnumerable<IFormFile>`uygulayan hedef türlere bağlanır.

Varsayılan kaynak doğru değilse, kaynağı belirtmek için aşağıdaki özniteliklerden birini kullanın:

* [`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -sorgu dizesinden değerleri alır. 
* [`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -rota verilerinden değerleri alır.
* [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -postalanan Form alanlarındaki değerleri alır.
* [`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -istek gövdesinden değerleri alır.
* [`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -http başlıklarındaki değerleri alır.

Bu öznitelikler:

* Model özelliklerine tek tek eklenir (model sınıfına değil), aşağıdaki örnekte olduğu gibi:

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* İsteğe bağlı olarak oluşturucuda bir model adı değeri kabul edin. Bu seçenek, özellik adının istekteki değerle eşleşmemesi durumunda sağlanır. Örneğin, istekteki değer aşağıdaki örnekte olduğu gibi adında bir tire olan bir üstbilgi olabilir:

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>[FromBody] özniteliği

Bir HTTP isteğinin gövdesinden özelliklerini doldurmak için `[FromBody]` özniteliğini bir parametreye uygulayın. ASP.NET Core çalışma zamanı, gövdeyi bir giriş biçimlendirici 'ya okumaktan sorumlu olarak temsil eder. Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.

Karmaşık bir tür parametresine `[FromBody]` uygulandığında, özelliklerine uygulanan herhangi bir bağlama kaynak özniteliği yok sayılır. Örneğin, aşağıdaki `Create` eylemi `pet` parametresinin gövdeden doldurulduğunu belirtir:

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

`Pet` sınıfı, `Breed` özelliğinin bir sorgu dizesi parametresinden doldurulduğunu belirtir:

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

Yukarıdaki örnekte:

* `[FromQuery]` özniteliği yok sayılır.
* `Breed` özelliği bir sorgu dizesi parametresinden doldurulmamış. 

Giriş biçimleri yalnızca gövdeyi okur ve bağlama kaynak özniteliklerini anlamayın. Gövdede uygun bir değer bulunursa, bu değer `Breed` özelliğini doldurmak için kullanılır.

Eylem yöntemi başına birden fazla parametreye `[FromBody]` uygulamayın. İstek akışı bir giriş biçimlendirici tarafından okunduktan sonra, diğer `[FromBody]` parametrelerini bağlamak için artık bir daha okunamaz.

### <a name="additional-sources"></a>Ek kaynaklar

Kaynak verileri, model bağlama sistemine *değer sağlayıcılara*göre sağlanır. Diğer kaynaklardan model bağlamaya yönelik verileri alan özel değer sağlayıcıları yazabilir ve kaydedebilirsiniz. Örneğin, tanımlama bilgileri veya oturum durumu verilerini isteyebilirsiniz. Yeni bir kaynaktan veri almak için:

* `IValueProvider`uygulayan bir sınıf oluşturun.
* `IValueProviderFactory`uygulayan bir sınıf oluşturun.
* Factory sınıfını `Startup.ConfigureServices`kaydedin.

Örnek uygulama, tanımlama bilgilerinden değerler alan bir [değer sağlayıcısı](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) ve [Factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) örneği içerir. `Startup.ConfigureServices`kayıt kodu aşağıda verilmiştir:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

Gösterilen kod, tüm yerleşik değer sağlayıcılarından sonra özel değer sağlayıcısını koyar.  Listenin ilk olması için, `Add`yerine `Insert(0, new CookieValueProviderFactory())` çağırın.

## <a name="no-source-for-a-model-property"></a>Model özelliği için kaynak yok

Varsayılan olarak, model özelliği için bir değer bulunmazsa model durumu hatası oluşturulmaz. Özelliği null veya varsayılan bir değer olarak ayarlanır:

* Null yapılabilir basit türler `null`olarak ayarlanır.
* Null yapılamayan değer türleri `default(T)`olarak ayarlanır. Örneğin, `int id` parametresi 0 olarak ayarlanır.
* Karmaşık türler için model bağlama, özellikleri ayarlamadan varsayılan oluşturucuyu kullanarak bir örnek oluşturur.
* Diziler, `byte[]` dizilerinin `null`olarak ayarlandığı durumlar dışında `Array.Empty<T>()`olarak ayarlanır.

Model özelliği için form alanlarında hiçbir şey bulunamadığında model durumunun geçersiz kılınmalıdır, [`[BindRequired]`](#bindrequired-attribute) özniteliğini kullanın.

Bu `[BindRequired]` davranışının, bir istek gövdesinde JSON veya XML verilerine değil, postalanan form verilerinden model bağlama için geçerli olduğunu unutmayın. İstek gövdesi verileri, [giriş formatlayıcıları](#input-formatters)tarafından işlenir.

## <a name="type-conversion-errors"></a>Tür dönüştürme hataları

Bir kaynak bulunursa ancak hedef türe dönüştürülemiyorsa, model durumu geçersiz olarak işaretlenir. Hedef parametresi veya özelliği, önceki bölümde belirtildiği gibi null veya varsayılan değer olarak ayarlanır.

`[ApiController]` özniteliğine sahip bir API denetleyicisinde, geçersiz model durumu otomatik HTTP 400 yanıtına neden olur.

Razor sayfasında, sayfayı bir hata iletisiyle yeniden görüntüleyin:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

İstemci tarafı doğrulama, aksi durumda Razor Pages bir forma gönderilemeyen hatalı verileri yakalar. Bu doğrulama, önceki vurgulanmış kodu tetiklemeyi zorlaştırır. Örnek uygulama, teslim **tarihi** alanına hatalı veri yerleştiren ve formu Gönderen **Geçersiz tarih içeren bir Gönder** düğmesi içerir. Bu düğme, veri dönüştürme hataları oluştuğunda sayfanın yeniden görüntülenmesine yönelik kodun nasıl çalıştığını gösterir.

Sayfa önceki kodla yeniden görüntülendiğinde, form alanında geçersiz giriş gösterilmez. Bunun nedeni model özelliğinin null ya da varsayılan bir değer olarak ayarlanmış olmasından kaynaklanır. Geçersiz giriş bir hata iletisinde görüntülenir. Ancak form alanındaki hatalı verileri yeniden görüntülemek istiyorsanız, model özelliğini bir dize haline getirmeyi ve veri dönüştürmeyi el ile gerçekleştirmeyi düşünün.

Tür dönüştürme hatalarının model durumu hatalarına neden olmasını istemiyorsanız aynı strateji önerilir. Bu durumda model özelliğini bir dize yapın.

## <a name="simple-types"></a>Basit türler

Model cildin kaynak dizeleri dönüştürebileceğiniz basit türler aşağıdakileri içerir:

* [Boole değeri](xref:System.ComponentModel.BooleanConverter)
* [Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)
* [Char](xref:System.ComponentModel.CharConverter)
* [Hem](xref:System.ComponentModel.DateTimeConverter)
* [Türünde](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Kategori](xref:System.ComponentModel.DecimalConverter)
* [Çift](xref:System.ComponentModel.DoubleConverter)
* [Yardımının](xref:System.ComponentModel.EnumConverter)
* ['İni](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Sunuculu](xref:System.ComponentModel.SingleConverter)
* [TimeSpan](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [Uri](xref:System.UriTypeConverter)
* [Sürüm](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Karmaşık türler

Karmaşık bir türün bağlanması için ortak bir varsayılan Oluşturucusu ve ortak yazılabilir özellikleri olmalıdır. Model bağlama gerçekleştiğinde, sınıf ortak varsayılan Oluşturucu kullanılarak oluşturulur. 

Karmaşık türün her özelliği için model bağlama, ad modeli ön eki için kaynakları arar *. property_name*. Hiçbir şey bulunamazsa, ön ek olmadan yalnızca *property_name* arar.

Bir parametreye bağlama için, önek parametre adıdır. `PageModel` public özelliğine bağlama için, önek ortak özellik adıdır. Bazı özniteliklerin, parametre veya özellik adının varsayılan kullanımını geçersiz kılabilmenizi sağlayan `Prefix` bir özelliği vardır.

Örneğin, karmaşık türün aşağıdaki `Instructor` sınıfı olduğunu varsayalım:

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Önek = parametre adı

Bağlanacak model `instructorToUpdate`adlı bir parametredir:

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

Model bağlama, anahtar `instructorToUpdate.ID`kaynaklara bakarak başlar. Bu bulunamazsa, öneki olmayan `ID` arar.

### <a name="prefix--property-name"></a>Önek = Özellik adı

Bağlanacak model, denetleyicinin veya `PageModel` sınıfının `Instructor` adlı bir özelliktir:

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar. Bu bulunamazsa, öneki olmayan `ID` arar.

### <a name="custom-prefix"></a>Özel ön ek

Bağlanacak model `instructorToUpdate` adlı bir parametredir ve `Bind` özniteliği önek olarak `Instructor` belirtir:

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

Model bağlama, anahtar `Instructor.ID`kaynaklara bakarak başlar. Bu bulunamazsa, öneki olmayan `ID` arar.

### <a name="attributes-for-complex-type-targets"></a>Karmaşık tür hedefleri için öznitelikler

Karmaşık türlerin model bağlamasını denetlemek için birkaç yerleşik öznitelik mevcuttur:

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> Bu öznitelikler, gönderilen form verileri değer kaynağı olduğunda model bağlamayı etkiler. Bunlar, gönderilen JSON ve XML istek gövdelerini işleyen giriş formatlayıcıları 'nı etkilemez. Giriş biçimleri [Bu makalenin ilerleyen kısımlarında](#input-formatters)açıklanmıştır.
>
> Ayrıca bkz. [model doğrulamasında](xref:mvc/models/validation#required-attribute)`[Required]` özniteliği tartışması.

### <a name="bindrequired-attribute"></a>[BindRequired] özniteliği

, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir. Modelin özelliği için bağlama gerçekleşmemişse model bağlamasının model durumu hatası eklemesine neden olur. Örnek buradadır:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>[Bindhiç] özniteliği

, Yöntem parametrelerine değil yalnızca model özelliklerine uygulanabilir. Model bağlamasının model özelliğini değiştirmesini engeller. Örnek buradadır:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>[Bind] özniteliği

, Bir sınıfa veya yöntem parametresine uygulanabilir. Model bağlamasındaki bir modelin hangi özelliklerinin dahil edileceğini belirtir.

Aşağıdaki örnekte, herhangi bir işleyici veya eylem yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

Aşağıdaki örnekte, `OnPost` yöntemi çağrıldığında yalnızca `Instructor` modelin belirtilen özellikleri bağlanır:

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

`[Bind]` özniteliği, *oluşturma* senaryolarında fazla nakline karşı korumak için kullanılabilir. Dışlanan Özellikler null ya da boş değer olarak ayarlandığı için, düzenleme senaryolarında iyi çalışmaz. Fazla nakline karşı savunma için, `[Bind]` özniteliği yerine, görüntüleme modelleri önerilir. Daha fazla bilgi için bkz. fazla [nakil hakkında güvenlik NOI](xref:data/ef-mvc/crud#security-note-about-overposting).

## <a name="collections"></a>Koleksiyonlar

Basit türlerin koleksiyonları olan hedefler için model bağlama, *parameter_name* veya *property_name*ile eşleşmeleri arar. Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar. Örneğin:

* Bağlanacak parametrenin `selectedCourses`adlı bir dizi olduğunu varsayalım:

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

* Aşağıdaki biçim yalnızca form verilerinde desteklenir:

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* Önceki örnek biçimlerinin hepsi için model bağlama iki öğe dizisini `selectedCourses` parametresine geçirir:

  * Selectedkurslar [0] = 1050
  * Selectedkurslar [1] = 2000

  Alt simge numaralarını kullanan veri biçimleri (... [0]... [1]...) sıfırdan başlayarak sıralı olarak numaralandırıldıklarından emin olmalıdır. Alt simge numaralandırmasında boşluk varsa, boşluklardan sonraki tüm öğeler yoksayılır. Örneğin, alt simgeler 0 ve 1 yerine 0 ve 2 ise ikinci öğe yok sayılır.

## <a name="dictionaries"></a>Sözlükler

`Dictionary` hedefler için, model bağlama *parameter_name* veya *property_name*eşleşmelerini arar. Eşleşme bulunmazsa, ön ek olmadan desteklenen biçimlerden birini arar. Örneğin:

* Hedef parametrenin `selectedCourses`adlı bir `Dictionary<int, string>` olduğunu varsayalım:

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Postalanan form veya sorgu dizesi verileri aşağıdaki örneklerden birine benzeyebilir:

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

* Önceki örnek biçimlerinin hepsi için model bağlama iki öğenin sözlüğünü `selectedCourses` parametresine geçirir:

  * Selectedkurslar ["1050"] = "Chemistry"
  * Selectedkurslar ["2000"] = "Ekonomiks"

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a>Model bağlama yolu verileri ve sorgu dizeleri için Genelleştirme davranışı

ASP.NET Core yol değeri sağlayıcısı ve sorgu dizesi değer sağlayıcısı:

* Değerleri sabit kültür olarak değerlendirin.
* URL 'Lerin kültür sabiti olmasını bekler.

Buna karşılık, form verilerinden gelen değerler kültüre duyarlı bir dönüştürmeye gider. Bu, URL 'Lerin yerel ayarlarda paylaşılabilir olması için tasarımdır.

ASP.NET Core yol değeri sağlayıcısını ve sorgu dizesi değeri sağlayıcısını, kültüre duyarlı bir dönüşüme dönüştürmek için:

* <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory> 'den devralma
* [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) veya [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs) adresinden kodu kopyalayın
* Değer sağlayıcısı oluşturucusuna geçirilen [kültür değerini](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture) ile değiştirin
* MVC seçeneklerinde varsayılan değer sağlayıcı fabrikasını yeni bir değerle değiştirin:

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a>Özel veri türleri

Model bağlamanın işleyebileceği bazı özel veri türleri vardır.

### <a name="iformfile-and-iformfilecollection"></a>Iformfile ve ıformfilecollection

HTTP isteğine eklenen karşıya yüklenen dosya.  Ayrıca, birden çok dosya için `IEnumerable<IFormFile>` desteklenir.

### <a name="cancellationtoken"></a>CancellationToken

Zaman uyumsuz denetleyicilerde etkinliği iptal etmek için kullanılır.

### <a name="formcollection"></a>Form koleksiyonu

Postalanan form verilerinden tüm değerleri almak için kullanılır.

## <a name="input-formatters"></a>Giriş biçimleri

İstek gövdesindeki veriler JSON, XML veya başka bir biçimde olabilir. Model bağlama, bu verileri ayrıştırmak için belirli bir içerik türünü işlemek üzere yapılandırılmış bir *giriş biçimlendiricisi* kullanır. Varsayılan olarak, ASP.NET Core JSON verilerini işlemek için JSON tabanlı giriş formatlayıcıları içerir. Diğer içerik türleri için başka biçim ekleyebilirsiniz.

ASP.NET Core, [tüketen](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) özniteliğine bağlı olarak giriş formatlayıcıları seçer. Hiçbir öznitelik yoksa, [Content-Type üst bilgisini](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)kullanır.

Yerleşik XML girişi formatlayıcıları 'nı kullanmak için:

* `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketini yükler.

* `Startup.ConfigureServices`<xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> veya <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>çağırın.

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* `Consumes` özniteliğini, istek gövdesinde XML beklemeniz gereken denetleyici sınıflarına veya eylem yöntemlerine uygulayın.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Daha fazla bilgi için bkz. [XML serileştirme tanıtımı](/dotnet/standard/serialization/introducing-xml-serialization).

## <a name="exclude-specified-types-from-model-binding"></a>Belirtilen türleri model bağlamalarından Dışla

Model bağlama ve doğrulama sistemlerinin davranışı [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata)tarafından yönlendiriliyor. [Mvcoptions. Modelmetadatadetails sağlayıcılarına](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders)bir ayrıntı sağlayıcısı ekleyerek `ModelMetadata` özelleştirebilirsiniz. Yerleşik Ayrıntılar sağlayıcıları, belirtilen türler için model bağlamayı veya doğrulamayı devre dışı bırakmak üzere kullanılabilir.

Belirtilen türdeki tüm modellerdeki model bağlamayı devre dışı bırakmak için `Startup.ConfigureServices`bir <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> ekleyin. Örneğin, `System.Version`türündeki tüm modeller üzerinde model bağlamayı devre dışı bırakmak için:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

Belirtilen türdeki özelliklerde doğrulamayı devre dışı bırakmak için `Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> ekleyin. Örneğin, `System.Guid`türündeki özelliklerde doğrulamayı devre dışı bırakmak için:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a>Özel model ciltleri

Özel bir model cildi yazarak ve belirli bir hedef için seçmek üzere `[ModelBinder]` özniteliğini kullanarak model bağlamayı genişletebilirsiniz. [Özel model bağlama](xref:mvc/advanced/custom-model-binding)hakkında daha fazla bilgi edinin.

## <a name="manual-model-binding"></a>El ile model bağlama

Model bağlama, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> yöntemi kullanılarak el ile çağrılabilir. Yöntemi hem `ControllerBase` hem de `PageModel` sınıflarında tanımlanmıştır. Yöntem aşırı yüklemeleri, kullanılacak öneki ve değer sağlayıcısını belirtmenizi sağlar. Model bağlama başarısız olursa Yöntem `false` döndürür. Örnek buradadır:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a>[FromServices] özniteliği

Bu özniteliğin adı, bir veri kaynağı belirten model bağlama özniteliklerinin düzeniyle uyar. Ancak bir değer sağlayıcısından veri bağlama hakkında bilgi yoktur. [Bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısından bir türün bir örneğini alır. Amacı, yalnızca belirli bir yöntem çağrılırsa bir hizmete ihtiyacınız olduğunda Oluşturucu ekleme için bir alternatif sağlamaktır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
