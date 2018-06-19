---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API HTTP tanımlama bilgilerini | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071635"
---
<a name="http-cookies-in-aspnet-web-api"></a>ASP.NET Web API HTTP tanımlama bilgileri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu konu, HTTP tanımlama bilgilerini Web API'si gönderip açıklar.

## <a name="background-on-http-cookies"></a>Arka plan HTTP tanımlama bilgisi

Bu bölümde, tanımlama bilgileri HTTP düzeyinde nasıl uygulandığını kısa genel bakış sağlar. Ayrıntılar için başvurun [RFC 6265](http://tools.ietf.org/html/rfc6265).

Bir tanımlama bilgisi, bir sunucu HTTP yanıt olarak gönderir veri parçasıdır. İstemci (isteğe bağlı) tanımlama bilgisi depolar ve subsequet isteklerinde döndürür. Bu, istemci ve sunucu durumu paylaşmasını sağlar. Bir tanımlama bilgisi ayarlamak için sunucunun yanıtta bir Set-Cookie üstbilgisi içerir. Bir tanımlama bilgisi isteğe bağlı özniteliklere sahip bir ad-değer çifti biçimidir. Örneğin:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Özniteliklerle örnek aşağıda verilmiştir:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Bir tanımlama bilgisinin sunucuya döndürülmesi için istemci tanımlama bilgisi üstbilgisi içinde sonraki istekleri içerir.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Bir HTTP yanıtı birden çok Set-Cookie üst bilgiler içerebilir.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

İstemciyi tek bir tanımlama bilgisi üstbilgisi kullanarak çok sayıda tanımlama bilgisini döndürür.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Bir tanımlama bilgisinin süre ve kapsamını Set-Cookie üst bilgisindeki aşağıdaki öznitelikleri tarafından denetlenir:

- **Etki alanı**: hangi etki alanının tanımlama bilgisi alması gereken istemci söyler. Örneğin, etki alanı "example.com" ise, istemci her alt etki alanı example.com için tanımlama bilgisi döndürür. Belirtilmezse, kaynak sunucunun etki alanıdır.
- **Yol**: tanımlama bilgisinin etki alanı içinde belirtilen yol kısıtlar. Belirtilmezse, istek URI'si yol kullanılır.
- **Süresi dolmadan**: tanımlama bilgisinin süre sonu tarihini ayarlar. Süresi dolduğunda, istemci tanımlama bilgisi siler.
- **Max-Age**: tanımlama bilgisinin yaş üst sınırını ayarlar. En uzun geçerlilik süresi ulaştığında istemci tanımlama bilgisi siler.

Her iki `Expires` ve `Max-Age` ayarlanır, `Max-Age` önceliklidir. Hiçbiri ayarlanırsa, istemci geçerli oturumu sona erdiğinde tanımlama bilgisi siler. ("Oturum" tam anlamı kullanıcı aracısı tarafından belirlenir.)

Ancak, istemciler tanımlama bilgilerini yoksayabilirsiniz unutmayın. Örneğin, bir kullanıcı gizlilikle ilgili nedenlerden dolayı tanımlama bilgilerini devre dışı bırakabilirsiniz. Süresi dolacak veya depolanan tanımlama bilgisi sayısını sınırlamak için önce istemcileri tanımlama bilgilerini silebilir. Gizlilik nedenlerle istemciler genellikle "üçüncü taraf" tanımlama bilgilerini, kaynak sunucunun nerede etki alanıyla eşleşmeyen reddeder. Kısacası, sunucunun ayarlar tanımlama bilgilerini geri alamazsınız güvenmemelisiniz.

## <a name="cookies-in-web-api"></a>Web API'de tanımlama bilgileri

Bir HTTP yanıt olarak bir tanımlama bilgisi eklemek için oluşturma bir **CookieHeaderValue** tanımlama bilgisini temsil eden örneği. ' I çağırın **AddCookies** tanımlanan genişletme yöntemi **System.Net.Http. HttpResponseHeadersExtensions** tanımlama bilgisi eklemek için sınıfı.

Örneğin, aşağıdaki kod bir denetleyici eylemi içinde bir tanımlama bilgisi ekler:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Dikkat **AddCookies** bir dizi alır **CookieHeaderValue** örnekleri.

Bir istemci isteğinden tanımlama bilgileri ayıklamak için çağrı **GetCookies** yöntemi:

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue** oluşan bir koleksiyon içeren **CookieState** örnekleri. Her **CookieState** bir tanımlama bilgisi temsil eder. Almak için dizin oluşturucu yöntemini kullanın bir **CookieState** gösterildiği gibi ada göre.

## <a name="structured-cookie-data"></a>Yapılandırılmış tanımlama bilgisi verileri

Bunlar depolar kaç tanımlama bilgilerini birçok tarayıcılar sınırlamak&#8212;toplam sayısı hem etki alanı başına sayı. Bu nedenle, çok sayıda tanımlama bilgisini ayarlamak yerine tek bir çerez yapılandırılmış veri yerleştirin yararlı olabilir.

> [!NOTE]
> RFC 6265 tanımlama bilgisi veri yapısı tanımlamıyor.


Kullanarak **CookieHeaderValue** sınıfı, tanımlama bilgisi verileri için ad-değer çiftlerinin listesini iletebilir. Bu ad-değer çiftleri Set-Cookie üst bilgisindeki URL kodlanmış form verileri olarak kodlanır:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

Önceki kod şu Set-Cookie üstbilgisi üretir:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState** sınıfı, bir tanımlama bilgisinde istek iletisi alt değerleri okuma için bir dizin oluşturucu yöntemi sağlar:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Örnek: Ayarlamak ve ileti işleyicisi tanımlama bilgilerini alma

Önceki örneklerde Web API denetleyicisi içinde tanımlama bilgilerini kullanmak nasıl oluşturulacağını gösterir. Başka bir seçenek kullanmaktır [ileti işleyicileri](http-message-handlers.md). İleti işleyicileri denetleyicileri daha düzenindeki daha önce çağrılır. Bir ileti işleyicisini denetleyici istek erişmeden önce tanımlama bilgilerini istekten okumak veya denetleyicisi yanıt oluşturduktan sonra yanıta tanımlama bilgilerini ekleyin.

![](http-cookies/_static/image2.png)

Aşağıdaki kod, oturum kimliklerini oluşturmak için bir ileti işleyicisini gösterir. Oturum kimliği bir tanımlama bilgisinde depolanır. İşleyici istek oturum tanımlama bilgisi için denetler. İstek tanımlama bilgisi içermiyorsa, işleyicinin yeni bir oturum kimliği oluşturur. Her iki durumda da, oturum kimliği işleyici depolar **HttpRequestMessage.Properties** özellik paketi. Ayrıca HTTP yanıtına oturum tanımlama bilgisi ekler.

Bu uygulama, istemciden gelen oturum kimliği sunucu tarafından gerçekten verilmiş olduğunu doğrulamaz. Form kimlik doğrulaması olarak kullanmayın! Örnek HTTP tanımlama bilgisi yönetim göstermek için noktasıdır.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Bir denetleyici oturum kimliği alabilirsiniz **HttpRequestMessage.Properties** özellik paketi.

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
