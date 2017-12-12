---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: "Açık yeniden yönlendirme saldırılarına (C#) önleme | Microsoft Docs"
author: jongalloway
description: "Bu öğreticide, ASP.NET MVC uygulamalarınızı açık yeniden yönlendirme saldırılarına nasıl engelleyebilirsiniz açıklanmaktadır. Bu öğretici yapmış olduğunuz değişiklikler tartışılır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 97e0aacbf21914bf95f01019cf4dcc9e7ca1c4be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="preventing-open-redirection-attacks-c"></a>Engelleme açık yeniden yönlendirme saldırılarına (C#)
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> Bu öğreticide, ASP.NET MVC uygulamalarınızı açık yeniden yönlendirme saldırılarına nasıl engelleyebilirsiniz açıklanmaktadır. Bu öğreticide, ASP.NET MVC 3'te AccountController içinde yapılan değişiklikler tartışılır ve, mevcut ASP.NET MVC 1.0 ve 2 uygulamalarında bu değişiklikleri nasıl uygulayabilirsiniz gösterir.


## <a name="what-is-an-open-redirection-attack"></a>Açık bir yeniden yönlendirme saldırısı nedir?

İstek sorgu dizesi veya form verileri gibi aracılığıyla belirtilen bir URL'ye yönlendiren herhangi bir web uygulaması olası kullanıcıları bir dış, kötü amaçlı URL'sine yeniden yönlendirmek için değiştirilmiş. Bu oynama açık yeniden yönlendirme saldırısı olarak adlandırılır.

Uygulama mantığınızın bir belirtilen URL'ye yeniden yönlendirilen olduğunda yeniden yönlendirme URL'sini oynanmadığını doğrulamanız gerekir. AccountController varsayılan olarak ASP.NET MVC 1.0 ve ASP.NET MVC 2 için kullanılan oturum açma yeniden yönlendirme saldırılarına açmak için savunmasızdır. Neyse ki, ASP.NET MVC 3 Önizleme düzeltmeleri kullanmak için mevcut uygulamalarınızı güncelleştirmek kolaydır.

Güvenlik Açığı anlamak için oturum açma yeniden yönlendirme varsayılan ASP.NET MVC 2 Web uygulaması projede nasıl çalıştığını konumundaki bakalım. Bu uygulamada [Authorize] özniteliğine sahip bir denetleyici eylemi ziyaret çalışılıyor /Account/LogOn görünümüne yetkisiz kullanıcıların yönlendirir. Böylece bunlar başarıyla oturum açtıktan sonra kullanıcı ilk olarak istenen URL'ye yeniden döndürülebilecek /Account/LogOn bu yeniden yönlendirme returnUrl sorgu dizesi parametresi dahil edilir.

Aşağıdaki ekran görüntüsünde, oturum açmadığı zaman /Account/ChangePassword görünüme erişme girişimi /Account/LogOn için bir yeniden yönlendirme gelen sonuçları görebiliyor musunuz? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Şekil 01**: açık bir yeniden yönlendirme ile oturum açma sayfası

ReturnUrl sorgu dizesi parametresi doğrulanmamış olduğundan, bir saldırganın açık yeniden yönlendirme saldırının yürütmek için parametre herhangi bir URL adresi eklemesine değiştirebilirsiniz. Bunu göstermek için biz ReturnUrl parametresi değiştirebilirsiniz [http://bing.com](http://bing.com), böylece ortaya çıkan oturum açma URL'si/Account/oturum açma olur? ReturnUrl = http://www.bing.com/. Başarıyla siteye oturum açtıktan sonra biz yönlendirilirsiniz [http://bing.com](http://bing.com). Bu yeniden yönlendirme doğrulanmamış olduğundan, bunun yerine kullanıcının kandırarak girişiminde kötü amaçlı bir siteye işaret edebilir.

### <a name="a-more-complex-open-redirection-attack"></a>Daha karmaşık bir açık yeniden yönlendirme saldırısı

Bir saldırgan biz bizi karşı savunmasız yapan belirli bir Web uygulamasına oturum açmaya çalıştığınız bildiği için açık yeniden yönlendirme saldırılarına özellikle tehlikeli bir [kimlik avı saldırı](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Örneğin, bir saldırgan kötü amaçlı e-postalar Web sitesi kullanıcılara parolalarını yakalama girişimi gönderebilir. Nasıl bu NerdDinner sitede çalışır konumundaki bakalım. (Dinamik NerdDinner site açık yeniden yönlendirme saldırılarına karşı korumak için güncelleştirilmiş unutmayın.)

İlk olarak, bir saldırganın bize bir bağlantı, onların sahte sayfa yeniden yönlendirme içeren NerdDinner oturum açma sayfasında gönderir:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Dönüş URL'SİNİN word Yemeği "n" eksik nerddiner.com işaret unutmayın. Bu örnekte, bu saldırgan denetleyen bir etki alanıdır. Biz yukarıdaki bağlantıyı eriştiğinizde, biz yasal NerdDinner.com oturum açma sayfasına yönlendirilirsiniz.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Şekil 02**: açık bir yeniden yönlendirme NerdDinner oturum açma sayfası

Biz doğru oturum açtığınızda, ASP.NET MVC AccountController'ın oturum açma eylemi bize returnUrl querystring parametresinde belirtilen URL'ye yeniden yönlendirir. Bu durumda olan ve bu da saldırganın girdiği, URL olan [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). Özellikle saldırgan emin olmak dikkatli olarak geçtiğinden biz bunu farkına olasılığı yüksektir son derece watchful ki sürece onların sahte sayfa tam olarak yasal oturum açma sayfası gibi görünüyor. Bu oturum açma sayfasına ki yeniden oturum olduğunu isteyen bir hata iletisi içerir. Biçimsiz bize, biz bizim parola yanlış yazmış gerekir.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Şekil 03**: sahte NerdDinner oturum açma ekranı

Biz bizim kullanıcı adı ve parolayı yeniden yazın, sahte oturum açma sayfası bilgileri kaydeder ve bize yasal NerdDinner.com sitesine geri gönderir. Sahte oturum açma sayfasına doğrudan bu sayfaya yönlendirebilirsiniz şekilde bu noktada, NerdDinner.com site zaten bize, kimlik doğrulaması. Sonuç, saldırgan, kullanıcı adı ve parola varsa ve biz bunu onlara sağladık olduğunu farkında ' dir.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>AccountController oturum açma eylemi savunmasız kodda bakarak

Bir ASP.NET MVC 2 uygulamada oturum açma eylemi için kod aşağıda verilmiştir. Bir oturum açma başarılı olduğunda, bir yeniden yönlendirme denetleyicisi returnUrl döndürür unutmayın. Hiçbir doğrulama karşı returnUrl parametre gerçekleştirildiği görebilirsiniz.

**1 – ASP.NET MVC 2 oturum açma eylemde listeleme`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Şimdi ASP.NET MVC 3 oturum açma eyleme değişiklikleri bakalım. Adlı System.Web.Mvc.Url yardımcı sınıfı içinde yeni bir yöntemini çağırarak returnUrl parametreyi doğrulamak için bu kodu değiştirildi `IsLocalUrl()`.

**2 – ASP.NET MVC 3 oturum açma eylemde listeleme`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Dönüş URL parametresi System.Web.Mvc.Url yardımcı sınıfı yeni yöntemi çağrılarak doğrulamak için bu değiştirildi `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>ASP.NET MVC 1,0 ve MVC 2 koruma uygulamaları

Biz ASP.NET MVC 3 değişiklikleri bizim mevcut ASP.NET MVC 1.0 ve 2 uygulamalarında IsLocalUrl() yardımcı yöntem ekleme ve returnUrl parametreyi doğrulamak için oturum açma eylemi güncelleştirme yararlanabilir.

Gerçekte yalnızca bu doğrulama olarak System.Web.WebPages bir yöntemi çağırma UrlHelper IsLocalUrl() yöntemi, ASP.NET Web Pages uygulamaları tarafından da kullanılır.

**3 – ASP.NET MVC 3 UrlHelper IsLocalUrl() yönteminden listeleme`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

IsUrlLocalToHost yöntemi listeleme 4'te gösterildiği gibi gerçek doğrulama mantığını içerir.

**4 – IsUrlLocalToHost() yöntemi System.Web.WebPages RequestExtensions sınıfından listeleme**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Bizim ASP.NET MVC 1.0 veya 2 uygulama, AccountController IsLocalUrl() yöntemi ekleyeceğiz, ancak ayrı yardımcı sınıfı için mümkünse eklemek için önerilir. Böylece AccountController içinde çalışacak IsLocalUrl() ASP.NET MVC 3 sürümüne iki küçük değişiklikler yapacağız. Genel yöntemler denetleyicileriyle denetleyici eylemleri erişilebilir olduğundan ilk olarak, biz bunu ortak yönteminden özel bir yönteme değiştireceksiniz. İkinci olarak, biz URL konak uygulama ana bilgisayarı karşı denetler çağrısı değiştireceksiniz. Çağrı yerel bir RequestContext kullanmak yapar UrlHelper sınıfı alanındaki. Bu kullanmak yerine. RequestContext.HttpContext.Request.Url.Host, bu kullanacağız. Request.Url.Host. Aşağıdaki kod, ASP.NET MVC 1.0 ve 2 uygulamalarında denetleyici sınıfını ile kullanmak için değiştirilmiş IsLocalUrl() yöntemi gösterir.

**5 – IsLocalUrl() yöntemi, listeleme, bir MVC denetleyicisi sınıfı ile kullanılmak üzere değiştirilir**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

IsLocalUrl() yöntemi bulunduğundan, biz returnUrl parametreyi doğrulamak için oturum açma eylemden aşağıdaki kodda gösterildiği gibi çağırabilirsiniz.

**6 – returnUrl parametre doğrular güncelleştirilmiş oturum açma yöntemi listeleme**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Şimdi biz açık yeniden yönlendirme saldırının dış dönüş URL'si kullanarak oturum açması deneyerek test edebilirsiniz. / Account/oturum açma kullanalım? ReturnUrl http://www.bing.com/ yeniden =.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Şekil 04**: güncelleştirilmiş oturum açma eylemi test etme

Başarıyla oturum açtıktan sonra biz dış URL yerine Home/Index denetleyici eylemini yönlendirilir.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Şekil 05**: engellenmediğinden açık yeniden yönlendirme saldırısı

## <a name="summary"></a>Özet

Bir uygulama için URL parametre olarak geçirilen yeniden yönlendirme URL'lerini açık yeniden yönlendirme saldırılarına ortaya çıkabilir. Yeniden yönlendirme saldırılarına karşı korumak üzere kod şablonu içerir ASP.NET MVC 3 açın. Bu kodu bazı değişikliği 2 uygulamalar ve ASP.NET MVC 1.0 ile ekleyebilirsiniz. ASP.NET 1.0 ve 2 uygulamaları oturum açma yeniden yönlendirme saldırılarına karşı korumak için bir IsLocalUrl() yöntemi ekleyin ve oturum açma eylemi returnUrl parametresinde doğrulayın.
