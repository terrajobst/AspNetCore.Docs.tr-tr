---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: ASP.NET MVC yürütme işlemini anlama | Microsoft Docs
author: microsoft
description: ASP.NET MVC çerçevesi bir tarayıcı isteğini adım adım nasıl işlediğini öğrenin.
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 34699c314eb862e91ecba8831c5092af9d47dc70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823741"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>ASP.NET MVC yürütme işlemini anlama
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC çerçevesi bir tarayıcı isteğini adım adım nasıl işlediğini öğrenin.


Bir ASP.NET MVC tabanlı Web uygulama isteklerini ilk geçirmek aracılığıyla **UrlRoutingModule** HTTP modülü nesne. Bu modül, istek ayrıştırır ve yol seçimi gerçekleştirir. **UrlRoutingModule** nesnesini geçerli istek eşleşen ilk yönlendirme nesne seçer. (Uygulayan bir sınıf rota nesnedir **RouteBase**, ve genellikle bir örneğini **rota** sınıfı.) Yol yok eşleşiyorsa **UrlRoutingModule** nesne hiçbir şey yapmaz ve normal ASP.NET veya IIS istek işleme dönmesi istek sağlar.

Seçili **rota** nesnesi **UrlRoutingModule** nesnesi alır **IRouteHandler** ile ilişkili nesne **rota**nesne. Genellikle, bir MVC uygulamasında, bu örneği olacaktır **MvcRouteHandler**. **IRouteHandler** örneği oluşturan bir **IHttpHandler** geçirir ve nesne **IHttpContext** nesne. Varsayılan olarak, **IHttpHandler** MVC için örnek **MvcHandler** nesne. **MvcHandler** nesne sonra sonuçta isteğini işleyecek denetleyiciyi seçer.

> [!NOTE]
> Bir ASP.NET MVC Web uygulaması, IIS 7. 0'çalıştığında, MVC projeleri için hiçbir dosya adı uzantısı gereklidir. Ancak, IIS 6. 0'da, işleyici .mvc dosya adı uzantısı için ASP.NET ISAPI DLL eşlemenizi gerektirir.


ASP.NET MVC çerçevesi için giriş noktaları modülü ve işleyici var. Bunlar, aşağıdaki eylemleri gerçekleştirin:

- Bir MVC Web uygulamasında uygun denetleyiciyi seçin.
- Belirli bir denetleyici örneği alın.
- Denetleyicinin çağrı **yürütme** yöntemi.

Bir MVC Web projesi için yürütme aşamalarını listeler:

- Uygulama için ilk isteği alma 

    - Global.asax dosyasındaki **rota** nesneleri eklenir **RouteTable** nesne.
- Yönlendirme gerçekleştirin 

    - **UrlRoutingModule** modülü kullanır uyan ilk **rota** nesnesine **RouteTable** oluşturmak için koleksiyon **RouteData** ardından oluşturmak için kullandığı bir nesne bir **RequestContext** (**IHttpContext**) nesne.
- MVC istek işleyicisi oluşturma 

    - **MvcRouteHandler** nesnesi örneği oluşturan **MvcHandler** sınıfı ve geçirir **RequestContext** örneği.
- Denetleyici oluşturma 

    - **MvcHandler** nesne kullandığı **RequestContext** tanımlamak için örnek **IControllerFactory** nesne (genellikle bir örneğini  **DefaultControllerFactory** sınıf) ile denetleyici örneği oluşturulamadı.
- Yürütme denetleyicisi - **MvcHandler** örneği çağrıları s denetleyicisi **yürütme** yöntemi. |
- Eylemini çağır 

    - Çoğu denetleyicileri devralınacak **denetleyicisi** temel sınıfı. Bunu yapmak denetleyicileri için **ControllerActionInvoker** denetleyiciyle ilişkili nesne hangi eylem yöntemini çağırmak için denetleyici sınıfının belirler ve ardından bu yöntemi çağırır.
- Yürütme sonucu 

    - Tipik bir eylem yöntemi, kullanıcı girişi almak, uygun yanıt verileri hazırlama ve sonucu bir sonuç türü döndürerek yürütebilirsiniz. Yürütülebilir yerleşik sonuç türleri şunlardır: **ViewResult** (görünümü işleyen ve en sık kullanılan sonuç türü olan), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, ve **EmptyResult**.
