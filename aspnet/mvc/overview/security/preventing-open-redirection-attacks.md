---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: (C#) açık yeniden yönlendirme saldırılarını önleme | Microsoft Docs
author: jongalloway
description: Bu öğreticide, ASP.NET MVC uygulamalarında açık yeniden yönlendirme saldırılarını nasıl engelleyebilir açıklar. Bu öğretici, yapılan değişiklikler tartışılır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 27921e23d38d34332b81fb85dcc795c8f9ff0352
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375476"
---
<a name="preventing-open-redirection-attacks-c"></a>Açık yeniden yönlendirme saldırılarını önleme (C#)
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> Bu öğreticide, ASP.NET MVC uygulamalarında açık yeniden yönlendirme saldırılarını nasıl engelleyebilir açıklar. Bu öğreticide, ASP.NET MVC 3'te AccountController içinde yapılan değişiklikler tartışılır ve nasıl, mevcut bir ASP.NET MVC 1.0 ve 2 uygulamalarında bu değişiklikleri uygulayabilirsiniz gösterir.


## <a name="what-is-an-open-redirection-attack"></a>Açık yeniden yönlendirme saldırının nedir?

Sorgu dizesi veya form verileri gibi isteği aracılığıyla belirtilen bir URL'ye yönlendiren herhangi bir web uygulaması büyük olasılıkla kullanıcıların dış, kötü amaçlı bir URL'ye yönlendirilmesini bozuabilir. Bu izinsiz bir açık yeniden yönlendirme saldırısı olarak adlandırılır.

Uygulama mantığınızın bir belirtilen URL'ye yeniden yönlendirir olduğunda, yeniden yönlendirme URL'sini kurcalanmadığı doğrulamanız gerekir. AccountController varsayılan olarak, ASP.NET MVC 1.0 ve ASP.NET MVC 2 için kullanılan oturum açma yeniden yönlendirme saldırılarını açmak için savunmasızdır. Neyse ki, ASP.NET MVC 3 Önizleme düzeltmeleri kullanmak için mevcut uygulamalarınızı güncelleştirmek kolaydır.

Güvenlik Açığı anlamak için oturum açma yeniden yönlendirme varsayılan ASP.NET MVC 2 Web uygulaması projesinde işleyişi sırasında göz atalım. Bu uygulamada [Authorize] özniteliği olan bir denetleyici eylemi ziyaret etmeye çalışırken yetkisiz kullanıcıların /Account/LogOn görünüme yönlendirir. Böylece bunlar başarıyla oturum açtıktan sonra başta istenen URL'ye kullanıcı döndürülebilir /Account/LogOn bu yeniden yönlendirme returnUrl querystring parametresi dahil edilir.

Aşağıdaki ekran görüntüsünde oturum açmamış /Account/ChangePassword görünüme erişme girişimi /Account/LogOn için bir yeniden yönlendirme sonuçları görebilir? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Şekil 01**: bir açık yeniden yönlendirme ile oturum açma sayfası

ReturnUrl querystring parametresi doğrulanmamış olduğundan, bir saldırganın parametresine bir açık yeniden yönlendirme saldırı yürütmek için herhangi bir URL adresi eklemesine değiştirebilirsiniz. Bunu göstermek için biz ReturnUrl parametresi değiştirebilirsiniz [ http://bing.com ](http://bing.com), sonuçta elde edilen oturum açma URL'si/hesabı/oturum açma olur? ReturnUrl =<http://www.bing.com/>. Başarıyla siteye oturum açtıktan sonra biz yönlendirilirsiniz [ http://bing.com ](http://bing.com). Bu yeniden yönlendirmeyi doğrulanmamış olduğundan, bunun yerine kullanıcı kandırmaya dener kötü amaçlı bir siteye işaret edebilir.

### <a name="a-more-complex-open-redirection-attack"></a>Daha karmaşık bir açık yeniden yönlendirme saldırısı

Bir saldırganın bizi savunmasız yapan belirli bir Web uygulamasına oturum deniyoruz olduğunu bildiğinden açık yeniden yönlendirme saldırılarını özellikle tehlikeli bir [avı kurbanı](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Örneğin, bir saldırgan kötü amaçlı e-postaları Web sitesi kullanıcılara parolalarını yakalama girişimi gönderebilir. Nasıl bu NerdDinner sitesinde işe yarar bakalım. (Açık yeniden yönlendirme saldırılarına karşı korumaya yönelik Canlı NerdDinner site güncelleştirildiğini unutmayın.)

İlk olarak, bir saldırganın bize bir bağlantı oturum açma sayfasına yeniden yönlendirme, sahte sayfa içeren NerdDinner gönderir:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Dönüş URL'si word Akşam Yemeği "n" eksik nerddiner.com işaret ettiğini unutmayın. Bu örnekte, bu saldırgan denetleyen bir etki alanıdır. Biz yukarıdaki bağlantıya eriştiğinizde, biz yasal NerdDinner.com oturum açma sayfasına yönlendirilirsiniz.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Şekil 02**: bir açık yeniden yönlendirme ile NerdDinner oturum açma sayfası

Biz doğru şekilde oturum açtığında, ASP.NET MVC AccountController'ın oturum açma eylemi bize returnUrl sorgu dizesi parametresinde belirtilen URL yönlendirir. Bu durumda, saldırganın girdiği, URL'sini olduğu [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). Özellikle, saldırgan emin olmak dikkatli kaldırıldı çünkü biz bunu fark olmaz büyük olasılıkla son derece watchful duyuyoruz sürece, sahte sayfa gibi tam olarak yasal oturum açma sayfası arar. Bu oturum açma sayfası ki yeniden oturum, istenirken bir hata iletisi içerir. Biçimsiz bize, biz bizim parola yanlış yazmış gerekir.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Şekil 03**: sahte NerdDinner oturum açma ekranı

Biz bizim kullanıcı adı ve parolayı yeniden yazın, sahte oturum açma sayfası bilgileri kaydeder ve bize yasal NerdDinner.com siteye geri gönderir. Sahte oturum açma sayfasına doğrudan söz konusu sayfaya yönlendirebilirsiniz. Bu nedenle bu noktada, NerdDinner.com site zaten bize yapıldığını. Sonuç, saldırgan, kullanıcı adı ve parola sahiptir ve biz bunu onlara sağladık, farkında değildir ' dir.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Güvenlik açığı olan kodu AccountController oturum açma eyleminin bakarak

Bir ASP.NET MVC 2 uygulamasında oturum açma eyleminin kodu aşağıda gösterilmiştir. Başarılı bir oturum açtıktan sonra bir yeniden yönlendirme denetleyici için returnUrl döndürmediğine dikkat edin. Doğrulama karşı returnUrl parametresi gerçekleştirilmekte olduğunu görebilirsiniz.

**1 – ASP.NET MVC 2 oturum açma eylem listeleme `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Artık ASP.NET MVC 3 oturum açma eylemi değişiklikleri göz atalım. Adlı System.Web.Mvc.Url yardımcı sınıfta yeni bir yöntem çağırarak returnUrl parametreyi doğrulamak için bu kodu değiştirildi `IsLocalUrl()`.

**2 – ASP.NET MVC 3 oturum açma eylem listeleme `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Bu, System.Web.Mvc.Url bir yardımcı sınıfta yeni bir yöntem çağırarak dönüş URL parametresini doğrulamak üzere değiştirildi `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>MVC 2 ve ASP.NET MVC 1.0 koruyan uygulamalar

Biz ASP.NET MVC 3 değişiklikleri bizim mevcut ASP.NET MVC 1.0 ve 2 uygulamalarında IsLocalUrl() yardımcı yöntem ekleme ve güncelleştirme returnUrl parametreyi doğrulamak için oturum açma eylemi yararlanabilirsiniz.

Aslında bir yöntemde, bu doğrulama olarak System.Web.WebPages çağırma UrlHelper IsLocalUrl() yöntemi, ASP.NET Web Pages uygulamaları tarafından da kullanılır.

**3 – ASP.NET MVC 3 UrlHelper IsLocalUrl() yöntemden listeleme `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost yöntemi listeleme 4'te gösterildiği gibi gerçek Doğrulama mantığı içerir.

**4 – System.Web.WebPages RequestExtensions sınıfından IsUrlLocalToHost() yöntemi listeleme**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Bizim ASP.NET MVC 1.0 ya da 2 uygulama için AccountController IsLocalUrl() yöntemi ekleyeceğiz ancak mümkünse bir ayrı bir yardımcı sınıfı eklemek için önerilir. AccountController içinde çalışır böylece IsLocalUrl() ASP.NET MVC 3 sürümüne iki küçük değişiklikler yapacağız. Denetleyici eylemleri denetleyicileri genel yöntemleri erişilebildiğinden ilk olarak, ortak bir yöntemi özel bir yönteme değiştireceğiz. İkinci olarak, biz URL konağı uygulama konağı karşı denetler çağrı değiştireceksiniz. Çağrı yerel bir RequestContext kullanmak yapar alanındaki UrlHelper sınıfı. Bunun yerine. RequestContext.HttpContext.Request.Url.Host, bunu kullanacağız. Request.Url.Host. Aşağıdaki kod, 2 uygulamaları ve ASP.NET MVC 1.0 ile bir denetleyici sınıfı ile kullanmak için değiştirilmiş IsLocalUrl() yöntemi gösterir.

**5-IsLocalUrl() yöntemi, listeleme, bir MVC denetleyici sınıfı ile kullanılmak üzere değiştirilir**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

IsLocalUrl() yöntemi yerinde olduğuna göre aşağıdaki kodda gösterildiği gibi biz bunu returnUrl parametreyi doğrulamak için oturum açma eylemimiz çağırabilirsiniz.

**6 – returnUrl parametresini doğrular güncelleştirilmiş oturum açma yöntemini listeleme**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Şimdi biz bir açık yeniden yönlendirme saldırı dış dönüş URL'si kullanarak oturum açma girişiminde bulunarak test edebilirsiniz. /Account/LogOn kullanalım? ReturnUrl =<http://www.bing.com/> yeniden.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Şekil 04**: güncelleştirilmiş oturum açma eylemi test etme

Başarıyla oturum açtıktan sonra biz dış URL'nin yerine Home/Index denetleyici eylemini yönlendirilir.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Şekil 05**: engellenmediğinden açık yeniden yönlendirme saldırısına

## <a name="summary"></a>Özet

Yeniden yönlendirme URL'leri bir uygulama için URL'yi parametre olarak geçirildiğinde açık yeniden yönlendirme saldırılarını ortaya çıkabilir. ASP.NET MVC şablonu karşı korumak için kod içeren 3 yeniden yönlendirme saldırılarını açın. ASP.NET MVC 1.0 ve 2 uygulamaları bazı değişiklik ile bu kodu ekleyebilirsiniz. ASP.NET 1.0 ve 2 uygulamalarda oturum açma yeniden yönlendirme saldırılarına karşı korumak için IsLocalUrl() yöntemi ekleyin ve oturum açma eylemi returnUrl parametresinde doğrulayın.
