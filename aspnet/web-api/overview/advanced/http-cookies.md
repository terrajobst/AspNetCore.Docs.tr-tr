---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API'de HTTP tanımlama bilgileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 21ba186c11f39bbeedd1c320b98476ba13af27e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819166"
---
<a name="http-cookies-in-aspnet-web-api"></a>ASP.NET Web API'de HTTP tanımlama bilgileri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu konu, göndermek ve Web API'de HTTP tanımlama bilgileri almak açıklar.

## <a name="background-on-http-cookies"></a>Arka planda HTTP tanımlama bilgileri

Bu bölüm, tanımlama bilgisi HTTP düzeyinde nasıl uygulandığını kısa genel bakış sağlar. Ayrıntılar için başvurun [RFC 6265](http://tools.ietf.org/html/rfc6265).

Bir tanımlama bilgisi, HTTP yanıtında bir sunucuya gönderdiği verilerin bir parçasıdır. İstemci (isteğe bağlı) tanımlama bilgisi depolar ve subsequet isteklerinde döndürür. Bu, istemci ve sunucu durumu paylaşmak sağlar. Bir tanımlama bilgisi ayarlamak için sunucunun yanıtta bir Set-Cookie üst bilgisini içerir. Bir tanımlama bilgisinin biçimi isteğe bağlı öznitelikleri ile bir ad-değer çiftidir. Örneğin:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Özniteliklere sahip bir örnek aşağıda verilmiştir:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Bir tanımlama bilgisinin sunucuya döndürülmesi için istemcinin sonraki istekleri tanımlama bilgisi üstbilgisi içerir.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Bir HTTP yanıtı birden çok Set-Cookie üst bilgi içerebilir.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

İstemciyi tek bir tanımlama bilgisi üstbilgisi kullanarak çok sayıda tanımlama bilgisini döndürür.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Projenin kapsamına ve süresine bir tanımlama bilgisinin Set-Cookie üst bilgisindeki aşağıdaki öznitelikleri tarafından denetlenir:

- **Etki alanı**: etki alanı tanımlama bilgisi alması gereken istemci söyler. Örneğin, etki alanı "example.com" ise, istemci her alt etki alanının example.com tanımlama bilgisini döndürür. Belirtilmezse, kaynak sunucu etki alanıdır.
- **Yol**: tanımlama bilgisinin etki alanı içinde belirtilen yol kısıtlar. Belirtilmezse, istek URI'si yolu kullanılır.
- **Süresi dolmadan**: bir sona erme tarihi tanımlama bilgisi ayarlar. Belirtecin süresi dolduğunda, istemcinin tanımlama bilgisi siler.
- **Max-Age**: tanımlama bilgisinin yaş üst sınırını ayarlar. En uzun geçerlilik süresi ulaştığında istemci tanımlama bilgisi siler.

Her iki `Expires` ve `Max-Age` ayarlanması `Max-Age` önceliklidir. Ayarlanmış ise, geçerli oturumu sona erdiğinde istemci tanımlama bilgisi siler. ("Oturumu" tam anlamı kullanıcı aracısı tarafından belirlenir.)

Ancak, istemciler tanımlama bilgilerini yoksayabilirsiniz unutmayın. Örneğin, kullanıcı gizlilik nedenleriyle tanımlama bilgilerini devre dışı bırakabilirsiniz. Süresi dolacak veya depolanan tanımlama bilgisi sayısını sınırlamak için önce istemcileri tanımlama bilgilerini silebilirsiniz. Gizlilik nedeniyle istemciler genellikle "üçüncü taraf" tanımlama bilgilerini, kaynak sunucu etki alanı burada eşleşmiyor reddeder. Kısacası, sunucu tanımlama bilgilerini ayarlar geri alamazsınız güvenmemelisiniz.

## <a name="cookies-in-web-api"></a>Web API'de tanımlama bilgileri

Bir HTTP yanıtına bir tanımlama bilgisi eklemek için oluşturun bir **CookieHeaderValue** tanımlama bilgisini temsil eden örneği. Ardından çağırın **AddCookies** alanında tanımlanan genişletme yöntemi **System.Net.Http. HttpResponseHeadersExtensions** tanımlama bilgisi eklemek için sınıfı.

Örneğin, aşağıdaki kod, içinde bir denetleyici eylemi bir tanımlama bilgisi ekler:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Dikkat **AddCookies** dizisi alır **CookieHeaderValue** örnekleri.

Bir istemci isteğinde tanımlama bilgileri ayıklamak için çağrı **GetCookies** yöntemi:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** koleksiyonunu içeren **CookieState** örnekleri. Her **CookieState** bir tanımlama bilgisi temsil eder. Almak için dizin oluşturucu yöntemi kullanın. bir **CookieState** adıyla gösterildiği gibi.

## <a name="structured-cookie-data"></a>Yapılandırılmış bir tanımlama bilgisi verisi

Bunlar depolar kaç tanımlama bilgilerini çoğu tarayıcısı sınırlamak&#8212;toplam hem etki alanı sayısı. Bu nedenle, çok sayıda tanımlama bilgisini ayarlamak yerine tek bir tanımlama, yapılandırılmış veriler yerleştirmenin yararlı olabilir.

> [!NOTE]
> RFC 6265 tanımlama bilgisi veri yapısı tanımlamıyor.


Kullanarak **CookieHeaderValue** sınıfı, tanımlama bilgisi verisi için ad-değer çiftlerinin listesini iletebilir. Bu ad-değer çiftleri Set-Cookie üst bilgisindeki formu URL kodlanmış veriler olarak kodlanmış:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Önceki kod, şu Set-Cookie üst bilgisini üretir:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** sınıfı bir tanımlama bilgisinde istek iletisi alt değerleri okumak için bir dizin oluşturucu yöntemi sağlar:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Örnek: Ayarlayın ve bir ileti işleyicisi tanımlama bilgilerini alma

Önceki örneklerde, bir Web API denetleyicisi içinde tanımlama bilgilerini kullanma gösterdi. Başka bir seçenek kullanmaktır [ileti işleyicileri](http-message-handlers.md). İleti işleyicileri denetleyicileri daha işlem hattındaki daha önce çağrılır. Bir ileti işleyicisi isteği ulaşır denetleyicisi önce tanımlama bilgilerini istekten okuyun veya denetleyicisi yanıt oluşturduktan sonra yanıta tanımlama bilgileri ekleyin.

![](http-cookies/_static/image2.png)

Aşağıdaki kod, oturum kimliklerini oluşturmaya yönelik bir ileti işleyicisi gösterir. Oturum kimliği, bir tanımlama bilgisinde depolanır. Oturum tanımlama bilgisi için istek işleyicisi denetler. İstek tanımlama bilgisi içermiyorsa, işleyici, yeni bir oturum kimliği oluşturur. Her iki durumda da, oturum kimliği işleyici depolar **HttpRequestMessage.Properties** özellik paketi. Ayrıca HTTP yanıtı oturum tanımlama bilgisi ekler.

Bu uygulama, istemciden gelen oturum kimliği, sunucu tarafından gerçekten verildiğini doğrulamaz. Form kimlik doğrulaması olarak kullanmayın! HTTP tanımlama bilgisi yönetim noktası örneğin göstermektir.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Bir denetleyici oturum kimliği alabilirsiniz **HttpRequestMessage.Properties** özellik paketi.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
