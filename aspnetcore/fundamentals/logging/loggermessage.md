---
title: ASP.NET core'da LoggerMessage ile yüksek performans günlüğü
author: guardrex
description: LoggerMessage yüksek performanslı günlük kaydı senaryoları için daha az nesne ayırma işlemleri gerektiren önbelleğe temsilci oluşturmak için kullanmayı öğrenin.
ms.author: riande
ms.date: 11/03/2017
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: a0080a20fed2d8fc295e55822c11d5731c6910ca
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207517"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>ASP.NET core'da LoggerMessage ile yüksek performans günlüğü

Tarafından [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) özellikleri daha az nesne ayırma işlemleri gerektiren önbelleğe temsilcileri oluşturup azaltılmış hesaplama ek yüküne karşılaştırdığınızda [Günlükçü genişletme yöntemleri](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), gibi `LogInformation`, `LogDebug`, ve `LogError`. Yüksek performanslı günlük kaydı senaryoları için kullanmak `LoggerMessage` deseni.

`LoggerMessage` Günlükçü genişletme yöntemleri performans aşağıdaki avantajları sağlar:

* Günlükçü genişletme yöntemleri gerektirir "kutulama (dönüştürme)" değer türleri gibi `int`, içine `object`. `LoggerMessage` Deseni statik kullanarak kutulama önler `Action` alanları ve kesin tür belirtilmiş parametrelere sahip genişletme yöntemleri.
* Günlükçü genişletme yöntemleri, her bir günlük iletisine yazılır ileti şablonunu (adlandırılmış bir biçim dizesi) ayrıştırma gerekir. `LoggerMessage` yalnızca bir şablon ileti tanımlandığında kez ayrıştırma gerektirir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulamayı gösterir `LoggerMessage` özelliklerle izleme sistemi temel bir teklif. Uygulama ekler ve bir bellek içi veritabanı kullanarak siler. Bu işlemler gerçekleşirken, günlük iletilerini kullanılarak oluşturulmuş `LoggerMessage` deseni.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[(LogLevel, EventID, String) tanımlamak](/dotnet/api/microsoft.extensions.logging.loggermessage.define) oluşturur bir `Action` temsilci bir ileti günlüğe kaydetme için. `Define` aşırı yüklemeleri adlandırılmış biçim dizesine (şablon) en fazla altı türü parametreleri geçirme izin verir.

Sağlanan dize `Define` yöntemi olan bir şablonu ve bir aradeğerlendirme dizesinde. Yer tutucuları türleri belirttiğiniz sırada doldurulur. Yer tutucu adlarını şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır. Bunlar, özellik adları içinde yapılandırılmış günlük verileri görür. Öneririz [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için. Örneğin, `{Count}`, `{FirstName}`.

Her günlük iletisi bir `Action` tarafından oluşturulan bir statik alanda tutulan `LoggerMessage.Define`. Örneğin, örnek uygulamayı dizin sayfası için bir GET isteği için bir günlük iletisine açıklamak için bir alan oluşturur (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

İçin `Action`, belirtin:

* Günlük düzeyi.
* Bir olay benzersiz tanımlayıcı ([EventID](/dotnet/api/microsoft.extensions.logging.eventid)) statik bir genişletme yöntemi adı.
* İleti şablonunu (biçim dizesi olarak adlandırılır). 

Örnek uygulama kümelerini dizin sayfası için bir istek:

* Günlük düzeyini `Information`.
* Olay Kimliği `1` adıyla `IndexPageRequested` yöntemi.
* İleti şablonu (biçim dizesi olarak adlandırılır) bir dize.

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Günlüğe kaydetme zenginleştirmek için olay kimliği ile sağlandığında yapılandırılmış günlük kaydı depoları olay adı kullanabilirsiniz. Örneğin, [Serilog](https://github.com/serilog/serilog-extensions-logging) olay adını kullanır.

`Action` Kesin türü belirtilmiş uzantısı yöntemiyle çağrılır. `IndexPageRequested` Yöntemi örnek uygulamada bir dizin sayfası GET isteği için bir ileti kaydeder:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` içinde günlükçü adı verilen `OnGetAsync` yönteminde *Pages/Index.cshtml.cs*:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Uygulamanın konsol çıkışını inceleyin:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Günlük ileti parametreleri geçirmek için en fazla altı tür statik alanı oluştururken tanımlayın. Örnek uygulama bir teklif tanımlayarak eklerken bir dize günlükleri bir `string` yazın `Action` alan:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Temsilcinin günlük iletisi şablonu, sağlanan türlerinden yer tutucu değerlerini alır. Örnek uygulamayı teklif parametrenin bulunduğu bir tırnak işareti eklemek için bir temsilci tanımlar. bir `string`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Bir teklif ekleme statik genişletme yöntemi `QuoteAdded`, teklif bağımsız değişkenin değerini alır ve buna ileten `Action` temsilci:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

Dizin sayfa sayfa modelinde (*Pages/Index.cshtml.cs*), `QuoteAdded` ileti günlüğe kaydetmek üzere çağrılır:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Uygulamanın konsol çıkışını inceleyin:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Örnek uygulama uygulayan bir `try` &ndash; `catch` teklif silinmek deseni. Bir bilgi iletisidir başarılı silme işlemi için günlüğe kaydedilir. Bir özel durum oluştuğunda bir hata iletisi bir silme işlemi için günlüğe kaydedilir. Başarısız silme işlemi için özel durum yığın izleme günlüğü iletisi içerir (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Özel durum temsilciye nasıl geçirildiğini unutmayın `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

Dizin sayfası için sayfa modelinde başarılı teklif silme işlemini çağırır `QuoteDeleted` Günlükçü yöntemi. Bir teklif silinmek üzere bulunamadığında, bir `ArgumentNullException` oluşturulur. Özel durum tarafından yakalanan `try` &ndash; `catch` deyimi ve çağırarak oturum `QuoteDeleteFailed` içinde Günlükçü metodunda `catch` blok (*Pages/Index.cshtml.cs*):

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Teklif başarıyla silindiğinde, uygulamanın konsol çıkışını inceleyin:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Teklif silme işlemi başarısız olduğunda uygulamanın konsol çıkışını inceleyin. Özel bir günlük iletisinde eklendiğini unutmayın:

```console
fail: LoggerMessageSample.Pages.IndexModel[5]
      => RequestId:0HL90M6E7PHK5:00000010 RequestPath:/ => /Index
      Quote delete failed (Id = 999)
System.ArgumentNullException: Value cannot be null.
Parameter name: entity
   at Microsoft.EntityFrameworkCore.Utilities.Check.NotNull[T](T value, String parameterName)
   at Microsoft.EntityFrameworkCore.DbContext.Remove[TEntity](TEntity entity)
   at Microsoft.EntityFrameworkCore.Internal.InternalDbSet`1.Remove(TEntity entity)
   at LoggerMessageSample.Pages.IndexModel.<OnPostDeleteQuoteAsync>d__14.MoveNext() in 
      <PATH>\sample\Pages\Index.cshtml.cs:line 87
```

## <a name="loggermessagedefinescope"></a>LoggerMessage.DefineScope

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) oluşturur bir `Func` temsilci tanımlamak için bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes). `DefineScope` aşırı yüklemeleri adlandırılmış biçim dizesine (şablon) en fazla üç türü parametreleri geçirme izin verir.

Olduğu gibi `Define` yöntem, sağlanan dize `DefineScope` yöntemi olan bir şablonu ve bir aradeğerlendirme dizesinde. Yer tutucuları türleri belirttiğiniz sırada doldurulur. Yer tutucu adlarını şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır. Bunlar, özellik adları içinde yapılandırılmış günlük verileri görür. Öneririz [Pascal casing](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için. Örneğin, `{Count}`, `{FirstName}`.

Tanımlayan bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes) günlük iletileri kullanarak bir dizi için uygulanacak [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) yöntemi.

Örnek uygulamanın bir **Tümünü Temizle** tüm veritabanı tırnak silme düğmesi. Tırnak işaretleri bunları kaldırmayı tarafından silinir birer güncelleştirir. Bir teklif silindiğinden, her zaman `QuoteDeleted` yöntemi Günlükçü üzerinde çağrılır. Bir günlük kapsamı bu günlük iletilerini eklenir.

Etkinleştirme `IncludeScopes` konsol Günlükçü bölümünde *appsettings.json*:

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

Günlük kapsamı oluşturmak için tutacak bir alan ekleyin. bir `Func` temsilci kapsamın. Örnek uygulama adında bir alan oluşturur `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Kullanım `DefineScope` temsilci oluşturmak için. Temsilci çağrıldığında, en fazla üç tür şablon bağımsız değişken olarak kullanım için belirtilebilir. Silinen tekliflerinin sayısını içeren bir ileti şablonu örnek uygulamayı kullanır (bir `int` türü):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Günlük iletisi için bir statik genişletme yöntemi sağlar. İleti şablonunda görünen adlandırılmış özellikleri için herhangi bir tür parametreleri içerir. Örnek uygulamayı alır bir `count` silmek için tırnak işaretleri döndürür ve `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Günlüğe kaydetme genişletme çağrıları kapsam sarmalayan bir `using` engelle:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Uygulamanın konsol çıkışında günlük iletilerini inceleyin. Aşağıdaki sonucu üç tırnak işaretleri dahil günlük kapsam iletinin silinmiş gösterir:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 1' Id = 2)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 2' Id = 3)
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:0000002E RequestPath:/ => /Index => All quotes deleted (Count = 3)
      Quote deleted (Quote = 'Quote 3' Id = 4)
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Günlüğe kaydetme](xref:fundamentals/logging/index)
