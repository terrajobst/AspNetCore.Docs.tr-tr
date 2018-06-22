---
title: ASP.NET Core LoggerMessage ile yüksek performanslı günlüğe kaydetme
author: guardrex
description: Yüksek performanslı günlük kaydı senaryoları için daha az nesne ayırmaları gerektiren alınabilir temsilciler oluşturmak için LoggerMessage kullanmayı öğrenin.
ms.author: riande
ms.date: 11/03/2017
uid: fundamentals/logging/loggermessage
ms.openlocfilehash: e952591bac29868d87d765820e88c74b50a1fe88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272441"
---
# <a name="high-performance-logging-with-loggermessage-in-aspnet-core"></a>ASP.NET Core LoggerMessage ile yüksek performanslı günlüğe kaydetme

Tarafından [Luke Latham](https://github.com/guardrex)

[LoggerMessage](/dotnet/api/microsoft.extensions.logging.loggermessage) özellikler oluşturmak daha az nesne ayırmaları gerektiren alınabilir Temsilciler ve azaltılmış hesaplama yükünü karşılaştırdığınızda [Günlükçü genişletme yöntemleri](/dotnet/api/Microsoft.Extensions.Logging.LoggerExtensions), gibi `LogInformation`, `LogDebug`, ve `LogError`. Yüksek performanslı günlük kaydı senaryoları için kullanmak `LoggerMessage` düzeni.

`LoggerMessage` Günlükçü genişletme yöntemleri aşağıdaki performans avantajları sunar:

* Günlükçü genişletme yöntemleri gerektirir "kutulama (dönüştürme)" değer türleri gibi `int`, içine `object`. `LoggerMessage` Düzeni statik kullanarak kutulama önler `Action` alanları ve kesin türü belirtilmiş parametrelerle genişletme yöntemleri.
* Günlükçü genişletme yöntemleri, her saat bir günlük iletisi yazılır iletisi şablonunu (adlandırılmış biçim dizesi) ayrıştırma gerekir. `LoggerMessage` yalnızca ileti tanımlandığında şablon kez ayrıştırma gerektirir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/loggermessage/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulamayı gösteren `LoggerMessage` izleme sistemi temel bir teklif özelliklerle. Uygulama ekler ve bir bellek içi veritabanı kullanarak teklifleri siler. Bu işlemler ortaya çıktığında günlük iletilerini kullanılarak oluşturulan `LoggerMessage` düzeni.

## <a name="loggermessagedefine"></a>LoggerMessage.Define

[(LogLevel, olay kimliği, dize) tanımlamak](/dotnet/api/microsoft.extensions.logging.loggermessage.define) oluşturur bir `Action` temsilci bir ileti günlüğe kaydetme için. `Define` aşırı yüklemeleri adlandırılmış biçim dizesine (şablonu) en fazla altı türü parametreleri geçirme izin verir.

Sağlanan dize `Define` yöntemdir bir şablon ve Ara değerli bir dize değil. Yer tutucu türleri belirttiğiniz sırayla doldurulur. Yer tutucu adları şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır. Bunlar özellik adları yapılandırılmış günlük verileri içinde işlevini görür. Öneririz [Pascal büyük/küçük harf](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için. Örneğin, `{Count}`, `{FirstName}`.

Her günlük iletisi bir `Action` tarafından oluşturulan statik bir alana tutulan `LoggerMessage.Define`. Örneğin, örnek uygulamayı dizin sayfası için bir GET isteği için bir günlük iletisi tanımlamak için bir alan oluşturur (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet1)]

İçin `Action`, belirtin:

* Günlük düzeyi.
* Bir benzersiz olay tanımlayıcısı ([EventID](/dotnet/api/microsoft.extensions.logging.eventid)) statik genişletme yöntemi adı.
* İleti şablonu (biçim dizesi olarak adlandırılır). 

Örnek uygulama kümelerinin dizin sayfası için bir istek:

* Günlük düzeyini `Information`.
* Olay Kimliği `1` adıyla `IndexPageRequested` yöntemi.
* (Biçim dizesi olarak adlandırılır) ileti şablona bir dize.

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet5)]

Günlüğe kaydetme zenginleştirmek için olay kimliği ile sağlandığında yapılandırılmış günlük depoları olay adı kullanabilirsiniz. Örneğin, [Serilog](https://github.com/serilog/serilog-extensions-logging) olay adını kullanır.

`Action` Kesin türü belirtilmiş genişletme yöntemi çağrılır. `IndexPageRequested` Yöntemi örnek uygulamasında bir dizin sayfası GET isteği için bir ileti kaydeder:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet9)]

`IndexPageRequested` Günlükçü olarak adlandırılan `OnGetAsync` yönteminde *Pages/Index.cshtml.cs*:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3)]

Uygulamanın konsol çıkışı inceleyin:

```console
info: LoggerMessageSample.Pages.IndexModel[1]
      => RequestId:0HL90M6E7PHK4:00000001 RequestPath:/ => /Index
      GET request for Index page
```

Bir günlük iletisine parametreleri geçirmek için statik alan oluştururken en fazla altı türünü tanımlar. Bir teklif tanımlayarak eklerken örnek uygulaması bir dize günlüklerinin bir `string` yazın `Action` alan:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet2)]

Temsilcinin günlük iletisi şablonunu sağlanan türlerinden yer tutucu değerlerini alır. Teklif parametresi olduğu bir teklif eklemek için bir temsilci örnek uygulaması tanımlar bir `string`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet6)]

Bir teklif eklemek için statik genişletme yöntemi `QuoteAdded`, teklif bağımsız değişken değeri alır ve buna ileten `Action` temsilci:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet10)]

Dizin sayfasının sayfa modelinde (*Pages/Index.cshtml.cs*), `QuoteAdded` ileti günlüğe kaydetmek üzere çağrılır:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet3&highlight=6)]

Uygulamanın konsol çıkışı inceleyin:

```console
info: LoggerMessageSample.Pages.IndexModel[2]
      => RequestId:0HL90M6E7PHK5:0000000A RequestPath:/ => /Index
      Quote added (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand')
```

Örnek uygulama uygulayan bir `try` &ndash; `catch` teklif silme işlemi için desen. Bir bilgi iletisidir başarılı silme işlemi için günlüğe kaydedilir. Bir özel durum bir hata iletisi silme işlemi için günlüğe kaydedilir. Başarısız silme işlemi için özel durum yığın izleme günlüğü iletisi içerir (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet3)]

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet7)]

Özel durum temsilciye nasıl geçirildiğini unutmayın `QuoteDeleteFailed`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet11)]

Dizin Sayfası sayfa modelinde başarılı teklif silme işlemini çağırır `QuoteDeleted` Günlükçü yöntemi. Bir teklif silinmek üzere bulunamadığında olduğunda bir `ArgumentNullException` oluşturulur. Özel durum tarafından yakalanan `try` &ndash; `catch` deyimi ve çağırarak günlüğe `QuoteDeleteFailed` Günlükçü yöntemi `catch` blok (*Pages/Index.cshtml.cs*):

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet5&highlight=14,18)]

Bir teklif başarıyla silindiğinde, uygulamanın konsol çıkışı inceleyin:

```console
info: LoggerMessageSample.Pages.IndexModel[4]
      => RequestId:0HL90M6E7PHK5:00000016 RequestPath:/ => /Index
      Quote deleted (Quote = 'You can avoid reality, but you cannot avoid the consequences of avoiding reality. - Ayn Rand' Id = 1)
```

Teklif silme işlemi başarısız olduğunda, uygulamanın konsol çıkışı inceleyin. Not: özel günlük iletisinde yer alır

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

[DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) oluşturur bir `Func` temsilci seçme tanımlamak için bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes). `DefineScope` aşırı yüklemeleri adlandırılmış biçim dizesine (şablonu) en fazla üç türü parametreleri geçirme izin verir.

İle olduğu gibi `Define` yöntemi, için sağlanan dize `DefineScope` yöntemdir bir şablon ve Ara değerli bir dize değil. Yer tutucu türleri belirttiğiniz sırayla doldurulur. Yer tutucu adları şablonundaki şablonlar arasında açıklayıcı ve tutarlı olmalıdır. Bunlar özellik adları yapılandırılmış günlük verileri içinde işlevini görür. Öneririz [Pascal büyük/küçük harf](/dotnet/standard/design-guidelines/capitalization-conventions) yer tutucu adları için. Örneğin, `{Count}`, `{FirstName}`.

Tanımlayan bir [oturum kapsamı](xref:fundamentals/logging/index#log-scopes) günlük iletileri kullanarak bir dizi uygulamak için [DefineScope(String)](/dotnet/api/microsoft.extensions.logging.loggermessage.definescope) yöntemi.

Örnek uygulaması olan bir **Tümünü Temizle** tüm veritabanı tırnaklar silme düğmesi. Tırnak işaretleri bunları kaldırarak silinir birer birer. Bir teklif silinir, her zaman `QuoteDeleted` yöntemi, üzerinde Günlükçü çağrılır. Bir günlük kapsamı bu günlük iletilerini eklenir.

Etkinleştirme `IncludeScopes` konsol Günlükçü bölümündeki *appsettings.json*:

[!code-csharp[](loggermessage/sample/appsettings.json?highlight=3-5)]

Bir günlük kapsamı oluşturmak için tutmak için bir alan ekleyebilmek bir `Func` kapsam için temsilci. Adlı bir alan örnek uygulaması oluşturur `_allQuotesDeletedScope` (*Internal/LoggerExtensions.cs*):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet4)]

Kullanım `DefineScope` temsilcisi oluşturmak için. Temsilci çağrıldığında en fazla üç tür şablon bağımsız değişken olarak kullanım için belirtilebilir. Örnek uygulaması silinen tekliflerinin sayısını içeren bir ileti şablonunu kullanır (bir `int` türü):

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet8)]

Günlük iletisi için bir statik genişletme yöntemi sağlar. Herhangi bir tür parametre görünür adlandırılmış özellikleri için ileti şablona dahil. Örnek uygulaması gereken bir `count` silmek için tırnak döndürür ve `_allQuotesDeletedScope`:

[!code-csharp[](loggermessage/sample/Internal/LoggerExtensions.cs?name=snippet12)]

Günlük uzantısı çağrıları kapsam sarmalayan bir `using` engelle:

[!code-csharp[](loggermessage/sample/Pages/Index.cshtml.cs?name=snippet4&highlight=5-6,14)]

Uygulamanın konsol çıkışı günlük iletilerini inceleyin. Aşağıdaki sonucu dahil günlük kapsam iletisiyle silinmiş üç teklifleri gösterir:

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
