---
title: ASP.NET Core Web API 'sinde JsonPatch
author: rick-anderson
description: ASP.NET Core Web API 'sindeki JSON Patch isteklerini nasıl işleyeceğinizi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: e57556e4b3fba55c6c187092593ffab4e31ee2d9
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727111"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a>ASP.NET Core Web API 'sinde JsonPatch

, [Tom Dykstra](https://github.com/tdykstra) ve [Kirk larkabağı](https://github.com/serpent5) tarafından

::: moniker range=">= aspnetcore-3.0"

Bu makalede, ASP.NET Core Web API 'sinde JSON Patch isteklerinin nasıl işleneceği açıklanır.

## <a name="package-installation"></a>Paket yüklemesi

`Microsoft.AspNetCore.Mvc.NewtonsoftJson` paketi kullanılarak JsonPatch desteği etkinleştirilmiştir. Bu özelliği etkinleştirmek için uygulamaların şunları yapmanız gerekir:

* [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükler.
* Projenin `Startup.ConfigureServices` yöntemini `AddNewtonsoftJson`çağrısı içerecek şekilde güncelleştirin:

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

`AddNewtonsoftJson`, MVC hizmeti kayıt yöntemleriyle uyumludur:

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a>JsonPatch, AddNewtonsoftJson ve System. Text. JSON
  
`AddNewtonsoftJson`, **Tüm** JSON içeriğini biçimlendirmek için kullanılan `System.Text.Json` tabanlı giriş ve çıkış formatlarını değiştirir. `Newtonsoft.Json`kullanarak `JsonPatch` için destek eklemek için, diğer formatlayıcıları değişmeden bırakarak, projenin `Startup.ConfigureServices` aşağıdaki gibi güncelleştirin:

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

Yukarıdaki kod, [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) öğesine ve aşağıdaki using deyimlerine yönelik bir başvuru gerektirir:

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a>PATCH HTTP istek yöntemi

PUT ve [Patch](https://tools.ietf.org/html/rfc5789) yöntemleri, var olan bir kaynağı güncelleştirmek için kullanılır. Bunlar arasındaki fark, PUT 'ın tüm kaynağı değiştirmesi, ancak düzeltme eki yalnızca değişiklikleri belirttiğinde.

## <a name="json-patch"></a>JSON yaması

[JSON yaması](https://tools.ietf.org/html/rfc6902) , bir kaynağa uygulanacak güncelleştirmelerin belirtilmesine yönelik bir biçimdir. JSON yama belgesinde bir dizi *işlem*vardır. Her işlem, bir dizi öğesi ekleme veya bir özellik değerini değiştirme gibi belirli bir değişiklik türünü tanımlar.

Örneğin, aşağıdaki JSON belgeleri bir kaynağı, kaynak için bir JSON yama belgesini ve düzeltme eki işlemlerini uygulamanın sonucunu temsil eder.

### <a name="resource-example"></a>Kaynak örneği

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>JSON Patch örneği

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

Önceki JSON 'da:

* `op` özelliği işlem türünü gösterir.
* `path` özelliği güncelleştirilecek öğeyi gösterir.
* `value` özelliği yeni değeri sağlar.

### <a name="resource-after-patch"></a>Düzeltme ekiyle sonra kaynak

Önceki JSON Patch belgesi uygulandıktan sonra kaynak şu şekildedir:

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

Bir kaynak için bir JSON Patch belgesi uygulanarak yapılan değişiklikler atomik: listedeki herhangi bir işlem başarısız olursa, listede hiçbir işlem uygulanmaz.

## <a name="path-syntax"></a>Yol sözdizimi

Bir işlem nesnesinin [Path](https://tools.ietf.org/html/rfc6901) özelliği düzeyler arasında eğik çizgi içeriyor. Örneğin: `"/address/zipCode"`.

Sıfır tabanlı dizinler, dizi öğelerini belirtmek için kullanılır. `addresses` dizisinin ilk öğesi `/addresses/0`. Bir dizinin sonuna `add` için dizin numarası yerine bir tire (-) kullanın: `/addresses/-`.

### <a name="operations"></a>İşlemler

Aşağıdaki tabloda, [JSON Patch belirtiminde](https://tools.ietf.org/html/rfc6902)tanımlanan desteklenen işlemler gösterilmektedir:

|Çalışma  | Notlar |
|-----------|--------------------------------|
| `add`     | Bir özellik veya dizi öğesi ekleyin. Var olan özellik için: set değeri.|
| `remove`  | Bir özellik veya dizi öğesi kaldırın. |
| `replace` | Aynı konumdaki `add` ve ardından `remove` ile aynı. |
| `move`    | Kaynaktaki değeri kullanarak `add` ve ardından hedefteki `remove` ile aynı. |
| `copy`    | Kaynaktaki değeri kullanarak hedefle `add` aynı. |
| `test`    | `path` = değeri `value`sağlanmışsa başarı durum kodunu döndürür.|

## <a name="jsonpatch-in-aspnet-core"></a>ASP.NET Core 'de JsonPatch

JSON düzeltme ekinin ASP.NET Core uygulanması, [Microsoft. AspNetCore. JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet paketinde sunulmaktadır.

## <a name="action-method-code"></a>Eylem yöntemi kodu

Bir API denetleyicisinde, JSON yaması için bir eylem yöntemi:

* `HttpPatch` özniteliğiyle açıklama eklenir.
* Genellikle `[FromBody]`ile bir `JsonPatchDocument<T>`kabul eder.
* Değişiklikleri uygulamak için düzeltme eki belgesinde `ApplyTo` çağırır.

Örnek buradadır:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Örnek uygulamadaki Bu kod, aşağıdaki `Customer` modeliyle çalışmaktadır.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Örnek eylem yöntemi:

* Bir `Customer`oluşturur.
* Düzeltme ekini uygular.
* Yanıtın gövdesinde sonucu döndürür.

 Gerçek bir uygulamada, kod veritabanı gibi bir mağazadan verileri alır ve düzeltme ekini uyguladıktan sonra veritabanını güncelleştirir.

### <a name="model-state"></a>Model durumu

Önceki eylem yöntemi örneği, parametrelerinden biri olarak model durumunu alan `ApplyTo` aşırı yüklemesini çağırır. Bu seçenekle, yanıtlardan hata iletileri alabilirsiniz. Aşağıdaki örnek, bir `test` işlemi için 400 Hatalı Istek yanıtı gövdesini gösterir:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Dinamik nesneler

Aşağıdaki eylem yöntemi örneği, dinamik bir nesne için bir düzeltme ekinin nasıl uygulanacağını gösterir.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Ekleme işlemi

* `path` bir dizi öğesine işaret ediyorsa: `path`tarafından belirtiden önce yeni bir öğe ekler.
* `path` bir özelliğe işaret ediyorsa: özellik değerini ayarlar.
* `path` varolmayan bir konuma işaret ediyorsa:
  * Yama yapılacak kaynak dinamik bir nesnedir: bir özellik ekler.
  * Yama yapılacak kaynak statik bir nesnese: istek başarısız olur.

Aşağıdaki örnek düzeltme eki belgesi, `CustomerName` değerini ayarlar ve `Orders` dizisinin sonuna bir `Order` nesnesi ekler.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Kaldırma işlemi

* `path` bir dizi öğesine işaret ediyorsa: öğeyi kaldırır.
* `path` bir özelliğe işaret ediyorsa:
  * Yayama kaynağı dinamik bir nesne ise: özelliğini kaldırır.
  * Yama yapılacak kaynak statik bir nesnese:
    * Özellik null atanabilir ise: null olarak ayarlar.
    * Özellik null atanamaz ise, `default<T>`olarak ayarlar.

Aşağıdaki örnek düzeltme eki belgesi null olarak `CustomerName` ve `Orders[0]`siler.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Değiştirme işlemi

Bu işlem, işlevsel olarak bir `remove` ve ardından bir `add`.

Aşağıdaki örnek düzeltme eki belgesi `CustomerName` değerini ayarlar ve `Orders[0]`yeni bir `Order` nesnesiyle değiştirir.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Taşıma işlemi

* `path` bir dizi öğesine işaret ediyorsa: `from` öğesini `path` öğesinin konumuna kopyalar, sonra `from` öğesinde bir `remove` işlemi çalıştırır.
* `path` bir özelliğe işaret ediyorsa: `from` özelliğinin değerini `path` özelliğine kopyalar, sonra `from` özelliğinde bir `remove` işlemi çalıştırır.
* `path` varolmayan bir özelliğe işaret ediyorsa:
  * Yama yapılacak kaynak statik bir nesnese: istek başarısız olur.
  * Düzeltme Eki uygulanacak kaynak dinamik bir nesnedir: `from` özelliğini `path`belirtilen konuma kopyalar, sonra `from` özelliğinde `remove` işlemini çalıştırır.

Aşağıdaki örnek düzeltme eki belgesi:

* `Orders[0].OrderName` değerini `CustomerName`olarak kopyalar.
* `Orders[0].OrderName` null olarak ayarlar.
* `Orders[0]`önce `Orders[1]` ' a gider.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Kopyalama işlemi

Bu işlem, son `remove` adımı olmadan bir `move` işlemiyle aynı işleve sahiptir.

Aşağıdaki örnek düzeltme eki belgesi:

* `Orders[0].OrderName` değerini `CustomerName`olarak kopyalar.
* `Orders[0]`önce `Orders[1]` bir kopyasını ekler.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Test işlemi

`path` tarafından belirtilen konumdaki değer `value`belirtilen değerden farklıysa, istek başarısız olur. Bu durumda, yama belgesindeki diğer tüm işlemler başka bir şekilde başarılı olsa bile, tüm yama isteği başarısız olur.

`test` işlem, yaygın olarak bir eşzamanlılık çakışması olduğunda bir güncelleştirmeyi engellemek için kullanılır.

`CustomerName` başlangıçtaki değeri "John" ise ve test başarısız olduğu için aşağıdaki örnek düzeltme eki belgesi etkisizdir:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Kodu alın

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([İndirme](xref:index#how-to-download-a-sample)).

Örneği test etmek için, uygulamayı çalıştırın ve aşağıdaki ayarlarla HTTP istekleri gönderin:

* URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* HTTP yöntemi: `PATCH`
* Üst bilgi: `Content-Type: application/json-patch+json`
* Gövde: *JSON proje klasöründen JSON Patch* belgesi örneklerinden birini kopyalayıp yapıştırın.

## <a name="additional-resources"></a>Ek kaynaklar

* [IETF RFC 5789 PATCH yöntemi belirtimi](https://tools.ietf.org/html/rfc5789)
* [IETF RFC 6902 JSON Patch belirtimi](https://tools.ietf.org/html/rfc6902)
* [IETF RFC 6901 JSON Patch yolu biçim belirtimi](https://tools.ietf.org/html/rfc6901)
* [JSON yama belgeleri](https://jsonpatch.com/). JSON yama belgeleri oluşturmak için kaynakların bağlantılarını içerir.
* [ASP.NET Core JSON Patch kaynak kodu](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu makalede, ASP.NET Core Web API 'sinde JSON Patch isteklerinin nasıl işleneceği açıklanır.

## <a name="patch-http-request-method"></a>PATCH HTTP istek yöntemi

PUT ve [Patch](https://tools.ietf.org/html/rfc5789) yöntemleri, var olan bir kaynağı güncelleştirmek için kullanılır. Bunlar arasındaki fark, PUT 'ın tüm kaynağı değiştirmesi, ancak düzeltme eki yalnızca değişiklikleri belirttiğinde.

## <a name="json-patch"></a>JSON yaması

[JSON yaması](https://tools.ietf.org/html/rfc6902) , bir kaynağa uygulanacak güncelleştirmelerin belirtilmesine yönelik bir biçimdir. JSON yama belgesinde bir dizi *işlem*vardır. Her işlem, bir dizi öğesi ekleme veya bir özellik değerini değiştirme gibi belirli bir değişiklik türünü tanımlar.

Örneğin, aşağıdaki JSON belgeleri bir kaynağı, kaynak için bir JSON yama belgesini ve düzeltme eki işlemlerini uygulamanın sonucunu temsil eder.

### <a name="resource-example"></a>Kaynak örneği

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>JSON Patch örneği

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

Önceki JSON 'da:

* `op` özelliği işlem türünü gösterir.
* `path` özelliği güncelleştirilecek öğeyi gösterir.
* `value` özelliği yeni değeri sağlar.

### <a name="resource-after-patch"></a>Düzeltme ekiyle sonra kaynak

Önceki JSON Patch belgesi uygulandıktan sonra kaynak şu şekildedir:

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

Bir kaynak için bir JSON Patch belgesi uygulanarak yapılan değişiklikler atomik: listedeki herhangi bir işlem başarısız olursa, listede hiçbir işlem uygulanmaz.

## <a name="path-syntax"></a>Yol sözdizimi

Bir işlem nesnesinin [Path](https://tools.ietf.org/html/rfc6901) özelliği düzeyler arasında eğik çizgi içeriyor. Örneğin: `"/address/zipCode"`.

Sıfır tabanlı dizinler, dizi öğelerini belirtmek için kullanılır. `addresses` dizisinin ilk öğesi `/addresses/0`. Bir dizinin sonuna `add` için dizin numarası yerine bir tire (-) kullanın: `/addresses/-`.

### <a name="operations"></a>İşlemler

Aşağıdaki tabloda, [JSON Patch belirtiminde](https://tools.ietf.org/html/rfc6902)tanımlanan desteklenen işlemler gösterilmektedir:

|Çalışma  | Notlar |
|-----------|--------------------------------|
| `add`     | Bir özellik veya dizi öğesi ekleyin. Var olan özellik için: set değeri.|
| `remove`  | Bir özellik veya dizi öğesi kaldırın. |
| `replace` | Aynı konumdaki `add` ve ardından `remove` ile aynı. |
| `move`    | Kaynaktaki değeri kullanarak `add` ve ardından hedefteki `remove` ile aynı. |
| `copy`    | Kaynaktaki değeri kullanarak hedefle `add` aynı. |
| `test`    | `path` = değeri `value`sağlanmışsa başarı durum kodunu döndürür.|

## <a name="jsonpatch-in-aspnet-core"></a>ASP.NET Core 'de JsonPatch

JSON düzeltme ekinin ASP.NET Core uygulanması, [Microsoft. AspNetCore. JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet paketinde sunulmaktadır. Paket, [Microsoft. AspnetCore. app](xref:fundamentals/metapackage-app) metapackage 'e dahildir.

## <a name="action-method-code"></a>Eylem yöntemi kodu

Bir API denetleyicisinde, JSON yaması için bir eylem yöntemi:

* `HttpPatch` özniteliğiyle açıklama eklenir.
* Genellikle `[FromBody]`ile bir `JsonPatchDocument<T>`kabul eder.
* Değişiklikleri uygulamak için düzeltme eki belgesinde `ApplyTo` çağırır.

Örnek buradadır:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Örnek uygulamadaki Bu kod, aşağıdaki `Customer` modeliyle çalışmaktadır.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Örnek eylem yöntemi:

* Bir `Customer`oluşturur.
* Düzeltme ekini uygular.
* Yanıtın gövdesinde sonucu döndürür.

 Gerçek bir uygulamada, kod veritabanı gibi bir mağazadan verileri alır ve düzeltme ekini uyguladıktan sonra veritabanını güncelleştirir.

### <a name="model-state"></a>Model durumu

Önceki eylem yöntemi örneği, parametrelerinden biri olarak model durumunu alan `ApplyTo` aşırı yüklemesini çağırır. Bu seçenekle, yanıtlardan hata iletileri alabilirsiniz. Aşağıdaki örnek, bir `test` işlemi için 400 Hatalı Istek yanıtı gövdesini gösterir:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Dinamik nesneler

Aşağıdaki eylem yöntemi örneği, dinamik bir nesne için bir düzeltme ekinin nasıl uygulanacağını gösterir.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Ekleme işlemi

* `path` bir dizi öğesine işaret ediyorsa: `path`tarafından belirtiden önce yeni bir öğe ekler.
* `path` bir özelliğe işaret ediyorsa: özellik değerini ayarlar.
* `path` varolmayan bir konuma işaret ediyorsa:
  * Yama yapılacak kaynak dinamik bir nesnedir: bir özellik ekler.
  * Yama yapılacak kaynak statik bir nesnese: istek başarısız olur.

Aşağıdaki örnek düzeltme eki belgesi, `CustomerName` değerini ayarlar ve `Orders` dizisinin sonuna bir `Order` nesnesi ekler.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Kaldırma işlemi

* `path` bir dizi öğesine işaret ediyorsa: öğeyi kaldırır.
* `path` bir özelliğe işaret ediyorsa:
  * Yayama kaynağı dinamik bir nesne ise: özelliğini kaldırır.
  * Yama yapılacak kaynak statik bir nesnese:
    * Özellik null atanabilir ise: null olarak ayarlar.
    * Özellik null atanamaz ise, `default<T>`olarak ayarlar.

Aşağıdaki örnek düzeltme eki belgesi null olarak `CustomerName` ve `Orders[0]`siler.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Değiştirme işlemi

Bu işlem, işlevsel olarak bir `remove` ve ardından bir `add`.

Aşağıdaki örnek düzeltme eki belgesi `CustomerName` değerini ayarlar ve `Orders[0]`yeni bir `Order` nesnesiyle değiştirir.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Taşıma işlemi

* `path` bir dizi öğesine işaret ediyorsa: `from` öğesini `path` öğesinin konumuna kopyalar, sonra `from` öğesinde bir `remove` işlemi çalıştırır.
* `path` bir özelliğe işaret ediyorsa: `from` özelliğinin değerini `path` özelliğine kopyalar, sonra `from` özelliğinde bir `remove` işlemi çalıştırır.
* `path` varolmayan bir özelliğe işaret ediyorsa:
  * Yama yapılacak kaynak statik bir nesnese: istek başarısız olur.
  * Düzeltme Eki uygulanacak kaynak dinamik bir nesnedir: `from` özelliğini `path`belirtilen konuma kopyalar, sonra `from` özelliğinde `remove` işlemini çalıştırır.

Aşağıdaki örnek düzeltme eki belgesi:

* `Orders[0].OrderName` değerini `CustomerName`olarak kopyalar.
* `Orders[0].OrderName` null olarak ayarlar.
* `Orders[0]`önce `Orders[1]` ' a gider.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Kopyalama işlemi

Bu işlem, son `remove` adımı olmadan bir `move` işlemiyle aynı işleve sahiptir.

Aşağıdaki örnek düzeltme eki belgesi:

* `Orders[0].OrderName` değerini `CustomerName`olarak kopyalar.
* `Orders[0]`önce `Orders[1]` bir kopyasını ekler.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Test işlemi

`path` tarafından belirtilen konumdaki değer `value`belirtilen değerden farklıysa, istek başarısız olur. Bu durumda, yama belgesindeki diğer tüm işlemler başka bir şekilde başarılı olsa bile, tüm yama isteği başarısız olur.

`test` işlem, yaygın olarak bir eşzamanlılık çakışması olduğunda bir güncelleştirmeyi engellemek için kullanılır.

`CustomerName` başlangıçtaki değeri "John" ise ve test başarısız olduğu için aşağıdaki örnek düzeltme eki belgesi etkisizdir:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Kodu alın

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([İndirme](xref:index#how-to-download-a-sample)).

Örneği test etmek için, uygulamayı çalıştırın ve aşağıdaki ayarlarla HTTP istekleri gönderin:

* URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* HTTP yöntemi: `PATCH`
* Üst bilgi: `Content-Type: application/json-patch+json`
* Gövde: *JSON proje klasöründen JSON Patch* belgesi örneklerinden birini kopyalayıp yapıştırın.

## <a name="additional-resources"></a>Ek kaynaklar

* [IETF RFC 5789 PATCH yöntemi belirtimi](https://tools.ietf.org/html/rfc5789)
* [IETF RFC 6902 JSON Patch belirtimi](https://tools.ietf.org/html/rfc6902)
* [IETF RFC 6901 JSON Patch yolu biçim belirtimi](https://tools.ietf.org/html/rfc6901)
* [JSON yama belgeleri](https://jsonpatch.com/). JSON yama belgeleri oluşturmak için kaynakların bağlantılarını içerir.
* [ASP.NET Core JSON Patch kaynak kodu](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
