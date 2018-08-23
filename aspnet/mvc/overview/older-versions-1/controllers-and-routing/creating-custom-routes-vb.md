---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Özel rotalar (VB) oluşturma | Microsoft Docs
author: microsoft
description: Özel rotalar için bir ASP.NET MVC uygulaması eklemeyi öğrenin. Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme konusunda bilgi edinin.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: cd7ad46161b3f44d915bc4b2fa201606b00d194a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754976"
---
<a name="creating-custom-routes-vb"></a>Özel rotalar oluşturma (VB)
====================
tarafından [Microsoft](https://github.com/microsoft)

> Özel rotalar için bir ASP.NET MVC uygulaması eklemeyi öğrenin. Bu öğreticide, Global.asax dosyasında varsayılan rota tablosu değiştirme konusunda bilgi edinin.


Bu öğreticide bir ASP.NET MVC uygulaması için özel bir yol ekleme konusunda bilgi edinin. Varsayılan rota tablosu ile özel bir rota Global.asax dosyasında değişiklik öğrenin.

ASP.NET MVC uygulamalarında varsayılan rota tablosu düzgün çalışır. Ancak, yönlendirme gereksinimleri özelleştirilmiş keşfedebilirsiniz. Bu durumda, özel bir yol oluşturabilirsiniz.

Örneğin, bir blog uygulaması oluşturmayı düşünün. Şuna gelen istekleri işlemek isteyebilirsiniz:

/ Arşiv/12-25-2009

Bir kullanıcı bu isteği girdiğinde, karşılık gelen blog girişine tarihe iade etmek istediğiniz 12/25/2009. Bu tür bir isteği işlemek için özel bir yol oluşturmanız gerekir.

Blog, /Archive/ gibi ara hangi istekleri işleme adlı yeni bir özel rota Global.asax dosyasında listeleniyor 1 içeren*girdisi tarihi*.

**1 - Global.asax (özel yönlendirme ile) listeleme**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

Yol tablosuna eklediğiniz rotaları sırası önemlidir. Sunduğumuz yeni özel Blog rota önce var olan varsayılan yol eklenir. Sıra ters, varsayılan yolu her zaman özel yol yerine çağrılmadığı.

Özel rota Blog/arşiv/ile başlayan herhangi bir istek eşleşir. Bu nedenle, aşağıdaki URL'ler tümünün eşleşecek:

- / Arşiv/12-25-2009

- / Arşiv/10-6-2004

- / Arşiv/apple

Özel rota, gelen istek arşiv adlı bir denetleyiciye eşler ve Entry() eylemi çağırır. Entry() yöntemi çağrıldığında, giriş tarihi entryDate adlı bir parametre olarak geçirilir.

Blog özel rota denetleyicisiyle listeleme 2'de kullanabilirsiniz.

**2 - ArchiveController.vb listeleme**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Listeleme 2 Entry() yöntemi DateTime türünde bir parametre kabul ettiğini dikkat edin. MVC çerçevesi bir DateTime değerine URL'den girdisi tarihi otomatik olarak dönüştürülmesi akıllı. URL'den giriş tarihi parametresi bir DateTime türüne dönüştürülemiyor, bir hata ortaya çıkar (bkz. Şekil 1).

**Şekil 1 - gelen parametre dönüştürme hatası**


[![Yeni Proje iletişim kutusu](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Şekil 01**: gelen parametre dönüştürme hatası ([tam boyutlu görüntüyü görmek için tıklatın](creating-custom-routes-vb/_static/image2.png))


## <a name="summary"></a>Özet

Bu öğreticinin amacı, bir özel yol nasıl oluşturacağınızı göstermek için oluştu. Blog girişleri temsil eder Global.asax dosyasındaki yol tablosuna bir özel yol eklemek öğrendiniz. Blog girişleri isteklerinde ArchiveController adlı bir denetleyici ve Entry() adlı bir denetleyici eylemi için eşleme ele almıştık.

> [!div class="step-by-step"]
> [Önceki](asp-net-mvc-controller-overview-vb.md)
> [İleri](creating-a-route-constraint-vb.md)
