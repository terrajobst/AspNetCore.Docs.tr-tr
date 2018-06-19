---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Özel yollar (VB) oluşturma | Microsoft Docs
author: microsoft
description: Bir ASP.NET MVC uygulaması için özel yollar eklemeyi öğrenin. Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872331"
---
<a name="creating-custom-routes-vb"></a>Özel yollar (VB) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bir ASP.NET MVC uygulaması için özel yollar eklemeyi öğrenin. Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme öğrenin.


Bu öğreticide, bir ASP.NET MVC uygulaması için özel bir yol ekleme konusunda bilgi edinin. Varsayılan rota tablosu ile özel bir rota Global.asax dosyasında değişiklik öğrenin.

ASP.NET MVC uygulamalarında varsayılan rota tablosu düzgün çalışır. Ancak, yönlendirme gereksinimlerini özelleştirilmiş fark edebilirsiniz. Bu durumda, özel bir yol oluşturabilirsiniz.

Örneğin, bir blog uygulaması oluşturduğunuzu düşünün. Şuna gelen istekleri işleyen isteyebilirsiniz:

/ Arşiv/12-25-2009

Bir kullanıcı bu isteği girdiğinde, karşılık gelen blog girişine tarihe döndürmek istediğiniz 25/12/2009. Bu tür bir isteği işlemek için özel bir yol oluşturmanız gerekir.

Blog, /Archive/ gibi görünen hangi istekleri işleme adlı yeni özel bir rota Global.asax dosyasında listeleniyor 1 içeren*giriş tarihi*.

**1 - Global.asax (özel rota ile) listeleme**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Yol tablosu ekleme yolları sıralama önemlidir. Bizim yeni özel Blog rota önce var olan varsayılan yol eklenir. Sırasını tersine, varsayılan yolu her zaman yerine özel rota çağrılmadığı.

Özel Blog rota başlatır/arşiv/herhangi bir istek eşleşir. Bu nedenle, aşağıdaki URL'ler tümünün eşleşecek:

- / Arşiv/12-25-2009

- / Arşiv/10-6-2004

- / Arşiv/apple

Özel rota gelen istek arşiv adlı bir denetleyiciye eşler ve Entry() eylemi çağırır. Giriş tarihi Entry() yöntemi çağrıldığında entryDate adlı bir parametre geçirilir.

2. listeleme denetleyicisiyle Blog özel rota kullanabilirsiniz.

**Listing 2 - ArchiveController.vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Listeleme 2 Entry() yönteminde DateTime türünde bir parametre kabul eden dikkat edin. MVC çerçevesi bir DateTime değerine URL'den giriş tarihi otomatik olarak dönüştürmek akıllıca olur. URL giriş tarihi parametresinden DateTime olarak dönüştürülemiyorsa bir hata oluşturulur (bkz: Şekil 1).

**Şekil 1 - parametre dönüştürme gelen hata**


[![Yeni Proje iletişim kutusu](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Şekil 01**: parametre dönüştürme gelen hata ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Özet

Özel rota nasıl oluşturabileceğinizi göstermek için bu öğreticinin amacı oluştu. Yol tablosu blog girdileri temsil eder Global.asax dosyasında özel bir rota eklemek öğrendiniz. Biz ArchiveController adlı bir denetleyici ve Entry() adlı bir denetleyici eylemi için blog girdileri isteklerinde eşlemek açıklanır.

> [!div class="step-by-step"]
> [Önceki](asp-net-mvc-controller-overview-vb.md)
> [sonraki](creating-a-route-constraint-vb.md)
