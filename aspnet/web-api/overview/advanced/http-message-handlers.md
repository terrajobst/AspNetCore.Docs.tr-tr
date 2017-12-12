---
uid: web-api/overview/advanced/http-message-handlers
title: "ASP.NET Web API HTTP ileti işleyicileri | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="http-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API HTTP ileti işleyicileri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

A *ileti işleyicisi* bir HTTP isteğini alır ve bir HTTP yanıtı döndüren bir sınıftır. İleti işleyicileri türetilen Özet **HttpMessageHandler** sınıfı.

Genellikle, bir dizi ileti işleyicileri zincirleme birlikte. İlk işleyici bir HTTP isteği aldığında, bir işlem yapar ve sonraki işleyicisi isteği verir. Belirli bir noktada yanıtı oluşturulur ve tekrar zincirinde geçer. Bu desen adlı bir *temsilci* işleyicisi.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Sunucu tarafı ileti işleyicileri

Sunucu tarafında Web API ardışık düzeni bazı yerleşik ileti işleyicileri kullanır:

- **HttpServer** istek ana bilgisayardan alır.
- **HttpRoutingDispatcher** Rotaya bağlı isteği gönderir.
- **HttpControllerDispatcher** bir Web API denetleyicisi isteği gönderir.

Özel işleyicileri ardışık düzene ekleyebilirsiniz. İleti işleyicileri HTTP iletileri (yerine denetleyici eylemleri) düzeyinde çalışır arası kesme sorunları için iyidir. Örneğin, bir ileti işleyicisi olabilir:

- Okuma veya istek üstbilgileri değiştirin.
- Bir yanıt üstbilgisi yanıtlarını ekleyin.
- Denetleyici düşmeden önce istekleri doğrulayın.

Bu diyagramda ardışık düzenine eklenen iki özel işleyicileri gösterilmektedir:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> İstemci tarafında HttpClient ileti işleyicileri de kullanır. Daha fazla bilgi için bkz: [HttpClient ileti işleyicileri](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Özel ileti işleyicileri

Özel ileti işleyicisi yazma için türetilen **System.Net.Http.DelegatingHandler** ve geçersiz kılma **SendAsync** yöntemi. Bu yöntem aşağıdaki imzası vardır:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Yöntem alır bir **HttpRequestMessage** giriş ve zaman uyumsuz olarak döndürür olarak bir **httpresponsemessage öğesini**. Tipik bir uygulama şunları yapar:

1. İşlem istek iletisi.
2. Çağrı `base.SendAsync` iç işleyici isteği göndermek için.
3. İç işleyici bir yanıt iletisi döndürür. (Bu adım, zaman uyumsuz bağlıdır.)
4. Yanıtı işlemek ve çağırana döndürür.

Önemsiz bir örnek aşağıda verilmiştir:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Çağrı `base.SendAsync` zaman uyumsuz olarak çağrılır. İşleyici bu çağrısından sonra herhangi bir iş varsa, kullanmak **await** gösterildiği gibi anahtar.


Temsilci işleyici Ayrıca iç işleyici atlayın ve doğrudan yanıt oluşturun:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Bir temsilci çağırmadan yanıt işleyici oluşturur `base.SendAsync`, istek kalan ardışık düzenini atlar. (Bir hata yanıtı oluşturma) istek doğrulama için bir işleyici yararlı olabilir.

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Ardışık düzene bir işleyici ekleme

Sunucu tarafında bir ileti işleyicisini eklemek, ekleyebilirsiniz **HttpConfiguration.MessageHandlers** koleksiyonu. Projeyi oluşturmak için "ASP.NET MVC 4 Web uygulaması" şablonu kullandıysanız, bu iç yapabileceğiniz **WebApiConfig** sınıfı:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

İleti işleyicileri görüntülendikleri aynı sırayla çağrılır **MessageHandlers** koleksiyonu. İç içe çünkü yanıt iletisi ters yönde dolaşır. Diğer bir deyişle, son yanıt iletisi ilk işleyicidir.

İç işleyicileri ayarlamanız gerekmez dikkat edin. Web API çerçevesi ileti işleyicileri otomatik olarak bağlanır.

Kullanıyorsanız [kendi kendine barındırma](../older-versions/self-host-a-web-api.md), bir örneğini oluşturmak **HttpSelfHostConfiguration** sınıfı ve işleyicileri ekleyin **MessageHandlers** koleksiyonu.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Şimdi özel ileti işleyicileri bazı örnekleri bakalım.

## <a name="example-x-http-method-override"></a>Örnek: X-HTTP-Method-Override

X-HTTP-Method-Override standart olmayan bir HTTP üstbilgisi ' dir. PUT veya silme gibi belirli HTTP istek türlerini gönderemiyor istemciler için tasarlanmıştır. Bunun yerine, istemci bir POST isteği gönderir ve istenen yöntemin X-HTTP-Method-Override üstbilgisini ayarlar. Örneğin:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

X-HTTP-Method-Override için destek ekler bir ileti işleyicisini şöyledir:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

İçinde **SendAsync** yöntemi, işleyici denetler istek iletisi bir POST isteği olup olmadığını, ve X-HTTP-Method-Override üstbilgi içerip içermediğini. Bu durumda, üstbilgi değeri doğrular ve istek yöntemini değiştirir. Son olarak, işleyici çağırması `base.SendAsync` sonraki işleyicisine ileti geçirmek için.

İstek ulaştığında **HttpControllerDispatcher** sınıfı, **HttpControllerDispatcher** güncelleştirilmiş isteği yöntemine dayalı istek yönlendirir.

## <a name="example-adding-a-custom-response-header"></a>Örnek: bir özel yanıt üstbilgisi ekleme

Her yanıt iletisi için bir özel üst bilgi ekler bir ileti işleyicisini şöyledir:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

İlk olarak, işleyici çağırması `base.SendAsync` iç ileti işleyicisi isteği geçirmek için. Bir yanıt iletisi iç işleyici döndürür, ancak bu nedenle zaman uyumsuz olarak kullanarak yapar bir **görev&lt;T&gt;**  nesnesi. Yanıt iletisi kadar kullanılamaz `base.SendAsync` zaman uyumsuz olarak tamamlar.

Bu örnekte **await** işlerini yapmak için anahtar sözcüğü zaman uyumsuz olarak sonra `SendAsync` tamamlar. .NET Framework 4.0 hedefliyorsanız kullanmak **görev**&lt;T&gt;**. Varsa** yöntemi:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Örnek: bir API anahtarı için denetimi

Bazı web Hizmetleri, istekte bir API anahtarı eklemek istemciler gerektirir. Aşağıdaki örnek, bir ileti işleyicisi geçerli bir API anahtarı için istekleri nasıl denetleyebilirsiniz gösterir:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Bu işleyici URI sorgu dizesinde API anahtarı arar. (Bu örnekte, anahtarın statik bir dize olduğunu varsayıyoruz. Gerçek bir uygulama büyük olasılıkla daha karmaşık doğrulama kullanırsınız.) Sorgu dizesi anahtar içeriyorsa, işleyici isteği iç işleyici geçirir.

İsteğin geçerli bir anahtar yoksa işleyicinin bir yanıt iletisi durumu 403, ile oluşturur. Yasak. Bu durumda, işleyici arama `base.SendAsync`, bu nedenle iç işleyici hiçbir zaman alan isteği de denetleyicisi çalışmaz. Bu nedenle, denetleyici gelen tüm istekleri geçerli bir API anahtarı olduğunu kabul edilebilir.

> [!NOTE]
> API anahtarı yalnızca belirli denetleyici eylemleri için geçerliyse, ileti işleyicisi yerine bir eylem filtresi kullanmayı düşünün. URI yönlendirme gerçekleştirildikten sonra eylem filtrelerini çalıştırın.


## <a name="per-route-message-handlers"></a>Rota başına ileti işleyicileri

İşleyiciler **HttpConfiguration.MessageHandlers** koleksiyonu uygulamak genel.

Alternatif olarak, rota tanımlarken özel bir rota ileti işleyicisi ekleyebilirsiniz:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

İstek URI'si "Route2" eşleşiyorsa, bu örnekte, istek için gönderilir `MessageHandler2`. Aşağıdaki diyagramda bu iki yolları için ardışık düzenini gösterir:

![](http-message-handlers/_static/image4.png)

Dikkat `MessageHandler2` varsayılan değiştirir **HttpControllerDispatcher**. Bu örnekte, `MessageHandler2` yanıtı oluşturur ve "Route2" hiçbir zaman bir denetleyiciye Git eşleşen ister. Bu, kendi özel uç noktası ile tüm Web API denetleyicisi mekanizması değiştirmenizi sağlar.

Alternatif olarak, bir rota başına ileti işleyicisi için temsilci olarak atayabilir miyim **HttpControllerDispatcher**, o bir denetleyiciye gönderir.

![](http-message-handlers/_static/image5.png)

Aşağıdaki kod, bu yolu yapılandırmak gösterilmektedir:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
