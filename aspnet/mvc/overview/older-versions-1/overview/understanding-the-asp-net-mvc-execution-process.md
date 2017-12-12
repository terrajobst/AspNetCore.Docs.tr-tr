---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: "ASP.NET MVC yürütme işlemi anlama | Microsoft Docs"
author: microsoft
description: "ASP.NET MVC çerçevesi adım adım bir tarayıcı isteğini nasıl işlediği hakkında bilgi edinin."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>ASP.NET MVC yürütme işlemi anlama
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC çerçevesi adım adım bir tarayıcı isteğini nasıl işlediği hakkında bilgi edinin.


Bir ASP.NET MVC tabanlı Web uygulama isteklerini ilk geçirmek aracılığıyla **UrlRoutingModule** bir HTTP modülü nesne. Bu modül isteği ayrıştırır ve rota seçimi gerçekleştirir. **UrlRoutingModule** nesne geçerli istek eşleşen ilk rota nesneyi seçer. (Uygulayan bir sınıf bir rota nesnesidir **RouteBase**, ve genellikle bir örneğini **rota** sınıfı.) Yol yok eşleşiyorsa **UrlRoutingModule** nesne hiçbir şey yapmaz ve normal ASP.NET veya IIS istek işleme geri isteği olanak tanır.

Seçilen **rota** nesnesi **UrlRoutingModule** nesnesi alır **IRouteHandler** ile ilişkili nesne **rota**nesnesi. Genellikle, bir MVC uygulamasında bu örneği olacaktır **MvcRouteHandler**. **IRouteHandler** örneği oluşturur bir **IHttpHandler** nesne ve geçirir **IHttpContext** nesnesi. Varsayılan olarak, **IHttpHandler** MVC için örnek **MvcHandler** nesnesi. **MvcHandler** nesne sonra sonuçta isteğini işleyecek denetleyiciyi seçer.

> [!NOTE]
> IIS 7. 0 ' bir ASP.NET MVC Web uygulaması çalıştığında, MVC projeleri için hiçbir dosya adı uzantısı gereklidir. Ancak, IIS 6. 0'da, ASP.NET ISAPI DLL .mvc dosya adı uzantısını eşlemek işleyici gerektirir.


Modül ve işleyici girişi ASP.NET MVC çerçevesi için noktalarıdır. Bunlar, aşağıdaki eylemleri gerçekleştirin:

- Uygun denetleyicisi bir MVC Web uygulaması seçin.
- Belirli bir denetleyici örneği edinin.
- Denetleyicinin çağrısı **yürütme** yöntemi.

Yürütme aşamaları bir MVC Web projesi için listeler:

- Uygulama için ilk isteği alma 

    - Global.asax dosyasındaki **rota** nesneleri eklenir **RouteTable** nesnesi.
- Yönlendirme gerçekleştirin 

    - **UrlRoutingModule** modülünü kullanan ilk eşleşen **rota** nesnesinde **RouteTable** oluşturmak için koleksiyon **RouteData** ardından oluşturmak için kullandığı nesne bir **RequestContext** (**IHttpContext**) nesnesi.
- MVC istek işleyicisi oluşturun 

    - **MvcRouteHandler** nesnesinin bir örneğini oluşturur **MvcHandler** sınıfı ve geçirir **RequestContext** örneği.
- Denetleyici oluşturma 

    - **MvcHandler** nesne kullanır **RequestContext** belirlemek için örnek **IControllerFactory** nesne (genellikle bir örneği  **DefaultControllerFactory** sınıfı) ile denetleyici örneği oluşturmak için.
- Execute denetleyicisi - **MvcHandler** örneği çağırır s denetleyicisi **yürütme** yöntemi. |
- Eylem çağırma 

    - Çoğu denetleyicileri devralınmalıdır **denetleyicisi** temel sınıfı. Bunu yapmak denetleyicileri için **ControllerActionInvoker** Denetleyici ile ilişkili olan nesne hangi eylemini yöntemi çağırmak için denetleyici sınıfının belirler ve bu yöntemi çağırır.
- Sonuç yürütme 

    - Tipik eylem yöntemi, kullanıcı girişi almak, uygun yanıtı verileri hazırlamak ve sonucu bir sonuç türü döndürerek sonra yürütün. Yürütülebilir yerleşik sonuç türleri şunlardır: **ViewResult** (hangi görünümü işleyen ve en sık kullanılan sonuç türü değil), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, ve **EmptyResult**.
