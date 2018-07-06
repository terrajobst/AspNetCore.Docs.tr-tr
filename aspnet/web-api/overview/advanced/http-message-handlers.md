---
uid: web-api/overview/advanced/http-message-handlers
title: ASP.NET Web API HTTP ileti işleyicileri | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 5694845d6f7f9d868c7b3f83785fddda863756d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821803"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API HTTP ileti işleyicileri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

A *ileti işleyicisi* HTTP isteği aldığında ve bir HTTP yanıtı döndüren bir sınıftır. İleti işleyicileri türetilen Özet **HttpMessageHandler** sınıfı.

Genellikle, bir dizi ileti işleyicileri zincirleme birlikte. İlk işleyici HTTP isteği aldığında, işlem yapar ve bir sonraki işleyici istek sağlar. Belirli bir noktada yanıt oluşturulur ve zincirinde döner. Bu düzen adı verilen bir *temsilci olarak görevlendirme* işleyici.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Sunucu tarafı ileti işleyicileri

Sunucu tarafında, Web API ardışık düzeni bazı yerleşik ileti işleyicileri kullanır:

- **HttpServer** istek ana bilgisayarından alır.
- **HttpRoutingDispatcher** yola göre isteği gönderir.
- **HttpControllerDispatcher** bir Web API denetleyicisine istek gönderir.

İşlem hattı için özel işleyicileri ekleyebilirsiniz. İleti işleyicileri, HTTP iletileri (yerine denetleyici eylemleri) düzeyinde çalışan çapraz kesme konuları için uygundur. Örneğin, bir ileti işleyicisi olabilir:

- Okuma veya istek üst bilgilerini değiştirin.
- Yanıt üst bilgisi yanıtlarını ekleyin.
- Denetleyici ulaşmadan önce istekleri doğrulayın.

Bu diyagramda, ardışık düzende eklenen iki özel işleyicileri gösterilmektedir:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> İstemci tarafında HttpClient ileti işleyicileri de kullanır. Daha fazla bilgi için [HttpClient ileti işleyicileri](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Özel ileti işleyicileri

Özel ileti işleyicisi yazmak için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi. Bu yöntem, aşağıdaki imzası vardır:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **HttpResponseMessage**. Tipik bir uygulaması şunları yapar:

1. İşlem istek iletisi.
2. Çağrı `base.SendAsync` iç işleyici için istek gönderebilirsiniz.
3. İç işleyici, bir yanıt iletisi döndürür. (Bu adım, zaman uyumsuz.)
4. Yanıta işlemek ve çağırana döndürmesi.

Önemsiz bir örnek aşağıda verilmiştir:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Çağrı `base.SendAsync` zaman uyumsuzdur. İşleyici, bu çağrıdan sonra herhangi bir iş varsa, kullanmak **await** gösterildiği gibi anahtar sözcüğü.


Bir temsilci işleyicisi, ayrıca iç işleyici atlamak ve doğrudan yanıt oluşturun:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Bir temsilci işleyicisi çağırmadan yanıtı oluşturur `base.SendAsync`, rest işlem hattının istek atlar. (Bir hata yanıtı oluşturma) istek doğrulama için bir işleyici yararlı olabilir.

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>İşlem hattı için bir işleyici ekleme

Sunucu tarafında bir ileti işleyicisi eklemek için ekleyebilirsiniz **HttpConfiguration.MessageHandlers** koleksiyonu. Projeyi oluşturmak için "ASP.NET MVC 4 Web uygulaması" şablonunu kullandıysanız, bu iç yapabileceğiniz **WebApiConfig** sınıfı:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

İleti işleyicileri görünürler aynı sırayla çağrılır **MessageHandlers** koleksiyonu. Yanıt iletisi içe yer olduğundan, diğer yönde hareket eder. Diğer bir deyişle, son ilk yanıt iletisi işleyicidir.

İç işleyicileri ayarlamanız gerekmez dikkat edin. Web API çerçevesi, ileti işleyicileri otomatik olarak bağlanır.

Eğer [kendi kendine barındırma](../older-versions/self-host-a-web-api.md), bir örneğini oluşturmak **HttpSelfHostConfiguration** sınıfı ve işleyicileri ekleyin **MessageHandlers** koleksiyonu.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Artık özel ileti işleyicileri bazı örneklere bakalım.

## <a name="example-x-http-method-override"></a>Örnek: X-HTTP-Method-Override

Standart olmayan bir HTTP üst bilgisi X-HTTP-Method-Override ' dir. PUT veya silme gibi belirli HTTP istek türlerini gönderilemiyor istemciler için tasarlanmıştır. Bunun yerine, istemci bir POST isteği gönderir ve istenen yöntemin X-HTTP-Method-Override üstbilgisini ayarlar. Örneğin:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

X-HTTP-Method-Override için destek ekleyen bir ileti işleyicisi şu şekildedir:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

İçinde **SendAsync** yöntemi, işleyici kontrol isteğine bir POST isteği olup ve X-HTTP-Method-Override üstbilgi içerip içermediğini. Bu durumda, üstbilgi değeri doğrular ve ardından istek yöntemi değiştirir. Son olarak, işleyici çağırır `base.SendAsync` iletiyi sonraki işleyicisine geçirilecek.

İstek ulaştığında **HttpControllerDispatcher** sınıfı **HttpControllerDispatcher** güncelleştirilmiş istek yöntemine dayalı isteği yönlendirir.

## <a name="example-adding-a-custom-response-header"></a>Örnek: bir özel yanıt üstbilgisi ekleme

Her yanıt iletisi için özel bir başlık ekler bir ileti işleyicisi şu şekildedir:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

İlk olarak, işleyici çağırır `base.SendAsync` iç ileti işleyicisi için isteği geçirilecek. İç işleyici bir yanıt iletisi döndürür, ancak bu nedenle zaman uyumsuz olarak kullanarak yaptığı bir **görev&lt;T&gt;**  nesne. Yanıt iletisi yüklenene kadar kullanılamaz `base.SendAsync` zaman uyumsuz olarak tamamlar.

Bu örnekte **await** işlerini yapmak için anahtar sözcüğü sonra zaman uyumsuz olarak `SendAsync` tamamlar. .NET Framework 4.0 hedefliyorsanız kullanın **görev**&lt;T&gt;**. ContinueWith** yöntemi:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Örnek: bir API anahtarı için denetimi

Bazı web Hizmetleri, istemciler kendi istekte bir API anahtarı içerecek şekilde gerektirir. Aşağıdaki örnek, bir ileti işleyicisi istekleri için geçerli bir API anahtarı nasıl denetleyebilirsiniz gösterir:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Bu işleyici, URI sorgu dizesinde API anahtarı arar. (Bu örnekte, bir statik dize anahtar olduğunu varsayıyoruz. Gerçek bir uygulaması büyük olasılıkla çok daha karmaşık Doğrulamalar kullanmanız gerekir.) Sorgu dizesinin anahtar içeriyorsa, işleyici isteği iç işleyiciye geçirir.

İsteğin geçerli bir anahtar yoksa, işleyici durumu 403, bir yanıt iletisi oluşturur. Yasak. Bu durumda, işleyici arama `base.SendAsync`, bu nedenle iç işleyici hiçbir zaman aldığı istek de denetleyicisi çalışmaz. Bu nedenle, denetleyici gelen tüm istekleri için geçerli bir API anahtarı olduğunu varsayabilirsiniz.

> [!NOTE]
> API anahtarı yalnızca belirli denetleyici eylemleri için geçerliyse, bir ileti işleyicisi yerine bir eyleme eylem filtresi kullanarak göz önünde bulundurun. Yönlendirme URI'si gerçekleştirildikten sonra eylem filtrelerini çalıştırın.


## <a name="per-route-message-handlers"></a>Rota başına ileti işleyicileri

İşleyicileri **HttpConfiguration.MessageHandlers** koleksiyon genel olarak uygulanır.

Alternatif olarak, rota tanımlarken belirli bir yol için bir ileti işleyicisi ekleyebilirsiniz:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

İstek URI'si "Route2" eşleşiyorsa, bu örnekte, istek için gönderilen `MessageHandler2`. Aşağıdaki diyagramda, bu iki yol için bir işlem hattı gösterilmektedir:

![](http-message-handlers/_static/image4.png)

Dikkat `MessageHandler2` varsayılan değiştirir **HttpControllerDispatcher**. Bu örnekte, `MessageHandler2` yanıtı oluşturur ve "Route2" hiçbir zaman bir denetleyiciye Git eşleşen ister. Bu, kendi özel uç nokta ile tüm Web API denetleyicisi mekanizması değiştirmenizi sağlar.

Alternatif olarak, bir rota başına ileti işleyicisini temsil edebilir **HttpControllerDispatcher**, daha sonra bir denetleyiciye gönderir.

![](http-message-handlers/_static/image5.png)

Aşağıdaki kod, bu rota yapılandırma işlemi gösterilmektedir:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
