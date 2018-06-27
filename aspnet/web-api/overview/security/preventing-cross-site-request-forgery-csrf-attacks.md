---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: ASP.NET Web API'de siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme | Microsoft Docs
author: MikeWasson
description: Siteler arası istek sahtekarlığı (CSRF) saldırı ve ASP.NET Web API'de anti-CSRF önlemlerini açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5e7b24c697e0bb37f388341abd89609c76f6b64c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961243"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>ASP.NET Web API'de siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Burada kötü amaçlı bir site olduğu kullanıcı şu anda oturum savunmasız bir siteye bir istek gönderir saldırının siteler arası istek sahteciliği (CSRF) değil

CSRF saldırı bir örneği burada verilmiştir:

1. İçine bir kullanıcı oturum `www.example.com` forms kimlik doğrulaması kullanma.
2. Sunucu kullanıcının kimliğini doğrular. Sunucudan gelen yanıtı bir kimlik doğrulama tanımlama bilgisi içerir.
3. Günlük olmadan, kullanıcının kötü amaçlı web sitesini ziyaret eder. Bu kötü amaçlı site aşağıdaki HTML formu içerir: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Form eylemi kötü amaçlı siteye değil savunmasız siteye yazılarını dikkat edin. CSRF "siteler arası" parçasıdır.
4. Kullanıcı gönder düğmesine tıklar. Tarayıcı kimlik doğrulama tanımlama bilgisi istekle içerir.
5. İstek, kullanıcının kimlik doğrulaması bağlamı ile sunucuda çalışır ve kimliği doğrulanmış bir kullanıcı yapmak için izin verilen herhangi bir şey yapabilirsiniz.

Bu örnekte form düğmesini kullanıcıya gerektirse de, kötü amaçlı sayfasını kolayca formu otomatik olarak göndermesi bir komut dosyası çalıştırma gibi verebilir. Ayrıca, SSL kullanarak, bunlar kötü amaçlı site "https://" istek gönderdikleri bile, CSRF saldırı engellemez.

Genellikle, tarayıcılar tüm ilgili tanımlama bilgilerini hedef web sitesine göndermek için CSRF saldırılarına karşı kimlik doğrulaması için tanımlama bilgileri kullanan web siteleri mümkün olabilir. Ancak, CSRF saldırıları tanımlama bilgisinden faydalanmakta için sınırlı değildir. Örneğin, temel ve Özet kimlik doğrulaması da savunmasız. Temel veya Özet kimlik bilgileriyle bir kullanıcı oturum sonra. oturumu sona kadar tarayıcı kimlik bilgileri otomatik olarak gönderir.

## <a name="anti-forgery-tokens"></a>Sahteciliğe karşı koruma belirteçleri

CSRF saldırılarını önlemeye yardımcı olmak için ASP.NET MVC olarak da bilinir sahteciliğe karşı koruma belirteçlerini kullanır *doğrulama belirteçler istemek*.

1. İstemci bir form içeren bir HTML sayfası ister.
2. Sunucunun yanıtta iki belirteçleri içerir. Bir simge bir tanımlama bilgisi olarak gönderilir. Diğer bir gizli form alanı yerleştirilir. Böylece bir saldırganın değerleri tahmin edilemez belirteçleri rastgele oluşturulur.
3. İstemci formu gönderdiğinde, sunucuya geri her iki simge göndermeniz gerekir. İstemcinin tanımlama bilgisi belirteci bir tanımlama bilgisi olarak gönderir ve formu belirteci içine form verileri gönderir. (Kullanıcı formu gönderdiğinde bir tarayıcı istemci otomatik olarak bunu yapar.)
4. Bir isteği her iki simge içermiyorsa, sunucu istek izin vermez.

Bir HTML formuna gizli bir form belirteci ile bir örneği burada verilmiştir:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Kötü amaçlı sayfası kaynak aynı ilkeleri nedeniyle kullanıcının belirteçleri okunamıyor çünkü sahteciliğe karşı koruma belirteçleri çalışır. ([Kaynak aynı ilkeleri](http://www.w3.org/Security/wiki/Same_Origin_Policy) birbirlerinin içeriğe erişmesini iki farklı sitelerde barındırılan belgelere engelleme. Bu nedenle önceki örnekte, kötü amaçlı sayfası istekleri example.com gönderebilir, ancak yanıt okuyamıyor.)

Kullanıcı oturum açtığında sonra CSRF saldırılarını engellemek, burada tarayıcı sessizce gönderir sahteciliğe karşı koruma belirteçleri ile herhangi bir kimlik doğrulama protokolünü kullanmak için kimlik bilgileri. Bu form kimlik doğrulaması gibi tanımlama bilgisi tabanlı kimlik doğrulama protokollerini içeren yanı sıra temel ve Özet kimlik doğrulaması gibi protokoller.

Sahteciliğe karşı koruma belirteçleri için tüm nonsafe yöntemleri (POST, PUT, DELETE) istemeniz gerekir. Ayrıca, güvenli yöntemleri (GET, HEAD) yan etkileri sahip olmadığından emin olun. CORS veya JSONP, gibi etki alanları arası desteğini etkinleştirirseniz, ayrıca, sonra da güvenli GET gibi ve bu da saldırganın olası hassas verileri okumak izin verme CSRF saldırılara karşı savunmasız olabilecek yöntemleridir.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>ASP.NET MVC sahteciliğe karşı koruma belirteçleri

Bir Razor sahteciliğe karşı koruma belirteçleri eklemek için kullanın **HtmlHelper.AntiForgeryToken** yardımcı yöntem:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Bu yöntem, gizli bir form alanı ekler ve ayrıca tanımlama bilgisi belirteci ayarlar.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF ve AJAX

Bir AJAX isteği JSON verilerini, HTML form verileri gönder çünkü form simgesi AJAX istekleri için bir sorun olabilir. Bir çözüm, özel bir HTTP üstbilgisi belirteçleri göndermektir. Aşağıdaki kod belirteçleri oluşturmak için Razor sözdizimini kullanır ve ardından bir AJAX isteği belirteçleri ekler. Belirteçleri çağırarak sunucuda oluşturulan **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

İsteği işlerken, istek üstbilgisi belirteçleri ayıklayın. ' I çağırın **AntiForgery.Validate** belirteçleri doğrulamak için yöntem. **Doğrulama** yöntemi belirteçleri geçerli değilse, bir özel durum oluşturur.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
