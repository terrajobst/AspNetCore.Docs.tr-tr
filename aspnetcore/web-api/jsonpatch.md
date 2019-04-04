---
title: ASP.NET Core Web API JsonPatch
author: tdykstra
description: Bir ASP.NET Core web API'si, JSON Patch isteklerinin nasıl işleneceğini öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: c5330f778ef202d366db5cdff5345d0db43a5c1b
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59013216"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a>ASP.NET Core Web API JsonPatch

tarafından [Tom Dykstra](https://github.com/tdykstra)

Bu makalede, bir ASP.NET Core web API'si, JSON Patch isteklerinin nasıl işleneceğini açıklar.

## <a name="patch-http-request-method"></a>PATCH HTTP istek yöntemi

KOY ve [düzeltme eki](https://tools.ietf.org/html/rfc5789) yöntemleri, mevcut bir kaynağı güncelleştirmek için kullanılır. Aralarındaki fark, düzeltme eki yalnızca değişiklikleri belirtirken PUT tüm kaynak değiştirir ' dir.

## <a name="json-patch"></a>JSON yaması

[JSON yaması](https://tools.ietf.org/html/rfc6902) bir kaynağa uygulanacak güncelleştirmeleri belirtmek için bir biçimidir. Bir dizi JSON yama belgesi sahip *operations*. Her bir işlemin belirli bir değişiklik türünü tanımlar, gibi bir dizi öğesine eklemek veya bir özellik değeri değiştirin.

Örneğin, aşağıdaki JSON belgelerini bir kaynak, kaynak ve patch işlemleri sonucu için JSON patch belgenin gösterir.

### <a name="resource-example"></a>Kaynak örneği

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>JSON patch örneği

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

Yukarıdaki JSON:

* `op` Özelliği işlem türünü belirtir.
* `path` Güncelleştirilecek öğe özelliği belirtir.
* `value` Özelliğinin yeni değeri sağlar.

### <a name="resource-after-patch"></a>Düzeltme eki sonra kaynak

Yukarıdaki JSON yama belgesi uyguladıktan sonra kaynak şöyledir:

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

Kaynak JSON yama belgesi uygulayarak yapılan değişiklikler atomiktir: listedeki herhangi bir işlem başarısız olursa, listedeki işlem uygulanır.

## <a name="path-syntax"></a>Yolu sözdizimi

[Yolu](http://tools.ietf.org/html/rfc6901) işlem nesnesi özelliğine düzeyleri arasında eğik çizgi sahiptir. Örneğin: `"/address/zipCode"`

Sıfır tabanlı dizin, dizi öğeleri belirtmek için kullanılır. İlk öğesi `addresses` dizi konumunda olacak `/addresses/0`. İçin `add` bir dizinin sonuna bir tire (-) yerine bir dizin numarasını kullanın: `/addresses/-`.

### <a name="operations"></a>İşlemler

Aşağıdaki tablo desteklenen gösterir işlemleri sınıfında tanımlandığı gibi [JSON Patch belirtimi](https://tools.ietf.org/html/rfc6902):

|Çalışma  | Notlar |
|-----------|--------------------------------|
| `add`     | Bir özellik ya da dizi öğesini ekleyin. Var olan bir özellik için: değeri ayarlayın.|
| `remove`  | Bir özellik ya da dizi öğesini kaldırın. |
| `replace` | Aynı `remove` ardından `add` aynı konumda. |
| `move`    | Aynı `remove` ardından kaynağından `add` değerini kullanarak bir kaynaktan bir hedefe. |
| `copy`    | Aynı `add` değerini kullanarak bir kaynaktan bir hedefe. |
| `test`    | Başarılı durum kodu döndürür değerini `path` sağlanan = `value`.|

## <a name="jsonpatch-in-aspnet-core"></a>ASP.NET core'da JsonPatch

JSON yaması ASP.NET Core uygulaması sağlanan [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet paketi. Paket dahil [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.

## <a name="action-method-code"></a>Eylem yöntemi kodu

API denetleyicisi içinde bir eylem yöntemi için JSON Patch:

* İle açıklanıyor `HttpPatch` özniteliği.
* Kabul eden bir `JsonPatchDocument<T>`, genellikle [FromBody] ile.
* Çağrıları `ApplyTo` üzerindeki değişiklikleri uygulamak için yama belgesi.

Örnek buradadır:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Bu örnek uygulama kodundan aşağıdakilerle çalışır `Customer` modeli.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Örnek eylem yöntemi:

* Oluşturur bir `Customer`.
* Düzeltme eki uygular.
* Yanıt gövdesinde sonucunu döndürür.

 Gerçek bir uygulamada kod bir veritabanı gibi bir depolama alanından verileri almak ve düzeltme eki uygulandıktan sonra veritabanı güncelleştirmesi.

### <a name="model-state"></a>Model durumu

Önceki eylem yöntemi örneği bir aşırı yüklemesini çağırır `ApplyTo` , parametrelerinden biri olarak model durumunu alır. Bu seçenek belirtilmişse, hata iletileri yanıtlarını alabilirsiniz. Aşağıdaki örnek, bir 400 Hatalı istek yanıt gövdesinin gösterir. bir `test` işlemi:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Dinamik nesneler

Eylem yöntemi aşağıda dinamik bir nesne için bir düzeltme eki uygulama işlemini gösterir.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Ekleme işlemi

* Varsa `path` bir dizi öğesine işaret eder: tarafından belirtilen bir önce yeni bir öğe ekler `path`.
* Varsa `path` gösteren bir özelliğe: özellik değeri ayarlar.
* Varsa `path` varolmayan bir konuma işaret eder:
  * Düzeltme eki kaynağa dinamik bir nesne ise: bir özellik ekler.
  * Kaynak düzeltme eki için statik bir nesneyse: istek başarısız olur.

Aşağıdaki örnek yama belgesi değerini ayarlar `CustomerName` ve ekler bir `Order` nesnesinin sonuna `Orders` dizisi.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Kaldırma işlemi

* Varsa `path` bir dizi öğesine işaret eder: öğeyi kaldırır.
* Varsa `path` işaret eden bir özellik için:
  * Kaynak düzeltme eki için dinamik bir nesne ise: özelliği kaldırır.
  * Kaynak düzeltme eki için statik bir nesne ise: 
    * Özellik boş değer atanabilir ise: null olarak ayarlar.
    * NULL olmayan bir özelliğidir, bu ayarlar `default<T>`.

Aşağıdaki örnek, düzeltme eki belge kümeleri `CustomerName` null ve silmeleri `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Değiştirme işlemi

Bu işlem işlevsel olarak aynıdır bir `remove` arkasından bir `add`.

Aşağıdaki örnek yama belgesi değerini ayarlar `CustomerName` ve değiştirir `Orders[0]`yeni bir `Order` nesne.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Taşıma işlemi

* Varsa `path` bir dizi öğesine işaret: kopyalar `from` konumunu öğesine `path` öğesi, daha sonra çalışan bir `remove` işlemi `from` öğesi.
* Varsa `path` gösteren bir özelliğe: kopyalar değerini `from` özelliğini `path` özelliği, daha sonra çalışan bir `remove` işlemi `from` özelliği.
* Varsa `path` işaret varolmayan bir özellik için:
  * Kaynak düzeltme eki için statik bir nesneyse: istek başarısız olur.
  * Yama kaynak dinamik bir nesne ise: kopyaları `from` özelliği tarafından belirtilen konuma `path`, sonra çalışan bir `remove` işlemi `from` özelliği.

Aşağıdaki örnek yama belgesi:

* Değerini kopyalar `Orders[0].OrderName` için `CustomerName`.
* Kümeleri `Orders[0].OrderName` null.
* Taşır `Orders[1]` için önce `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Kopyalama işlemi

Bu işlem işlevsel olarak aynıdır bir `move` son olmadan işlemi `remove` adım.

Aşağıdaki örnek yama belgesi:

* Değerini kopyalar `Orders[0].OrderName` için `CustomerName`.
* Bir kopyasını ekler `Orders[1]` önce `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Test işlemi

Bu konumdaki değeri tarafından belirtilmişse `path` sağlanan değerinden farklı `value`, istek başarısız olur. Bu durumda bile yama belgesindeki tüm diğer işlemler, aksi takdirde başarılı olabilir tüm PATCH isteği başarısız olur.

`test` İşlemi bir eşzamanlılık çakışması olduğunda, bir güncelleştirme önlemek için yaygın olarak kullanılır.

Aşağıdaki örnek yama belgesi etkisizdir başlangıç değeri oluşan `CustomerName` "John", test başarısız çünkü:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Kodu alma

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([Nasıl indirileceğini](xref:index#how-to-download-a-sample)).

Örnek test etmek için uygulamayı çalıştırın ve aşağıdaki ayarlarla HTTP istekleri göndermek için:

* URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* HTTP yöntemi: `PATCH`
* Üst bilgi: `Content-Type: application/json-patch+json`
* Gövdesi: JSON patch belge örneklerinden birini kopyalayıp *JSON* proje klasörü.

## <a name="additional-resources"></a>Ek kaynaklar

* [IETF RFC 5789 düzeltme eki yöntemi belirtimi](https://tools.ietf.org/html/rfc5789)
* [IETF RFC 6902 JSON Patch belirtimi](https://tools.ietf.org/html/rfc6902)
* [IETF RFC 6901 JSON Patch yolu biçim belirtimi](http://tools.ietf.org/html/rfc6901)
* [JSON yaması belgeleri](http://jsonpatch.com/). JSON yaması belgeleri oluşturmak için kaynakların bağlantılarını içerir.
* [ASP.NET Core JSON Patch kaynak kodu](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
