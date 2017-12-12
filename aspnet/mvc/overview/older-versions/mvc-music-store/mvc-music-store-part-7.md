---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "7. Kısım: Üyeliği ve yetkilendirme | Microsoft Docs"
author: jongalloway
description: "Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 7, üyelik ve yetkilendirme kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 064f2d6eca087fae8c796d1dde78d5079d3803ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-7-membership-and-authorization"></a>7. Kısım: Üyeliği ve yetkilendirme
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 7, üyelik ve yetkilendirme kapsar.


Bizim mağaza yöneticisi denetleyicisi sitemizi ziyaret eden herkes için şu anda erişilebilir değil. Bu site yöneticileri erişimi kısıtlamak için değiştirelim.

## <a name="adding-the-accountcontroller-and-views"></a>AccountController ve görünümler ekleme

Tam ASP.NET MVC 3 Web uygulaması şablonu ve ASP.NET MVC 3 boş Web uygulaması şablonu arasındaki tek fark, boş şablonu bir hesap denetleyicisi içermeyen ' dir. Bir hesap denetleyicisi tam ASP.NET MVC 3 Web uygulaması şablondan oluşturulan yeni bir ASP.NET MVC uygulamasında birkaç dosyaları kopyalayarak ekleyeceğiz.

Tam ASP.NET MVC 3 Web uygulaması şablonu kullanarak yeni bir ASP.NET MVC uygulaması oluşturma ve Projemizin aynı dizinlerde aşağıdaki dosyaları kopyalayın:

1. Denetleyicileri dizininde AccountController.cs kopyalama
2. Modelleri dizininde AccountModels kopyalama
3. Görünümleri dizin içinde bir hesap dizini oluşturun ve tüm dört görünümlerde kopyalayın

Denetleyici ve Model sınıfları için ad alanı ile MvcMusicStore başlayacak şekilde değiştirin. AccountController sınıfı MvcMusicStore.Controllers ad alanı kullanmalıdır ve AccountModels sınıfı MvcMusicStore.Models ad alanı kullanmanız gerekir.

*Not: Bu dosyalar, biz öğreticinin başında bizim site tasarımı dosyaları kopyalanan MvcMusicStore Assets.zip indirme mevcuttur. Üyelik dosyalar kodu dizininde bulunur.*

Güncellenen çözümü aşağıdaki gibi görünmelidir:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>ASP.NET yapılandırma sitesi olan bir yönetici kullanıcı ekleme

Biz yetkilendirme, Web sitesinin gerektirmeden önce biz erişimi olan bir kullanıcı oluşturma gerekir. Bir kullanıcı oluşturmak için en kolay yolu, yerleşik ASP.NET yapılandırması Web sitesini kullanmaktır.

ASP.NET yapılandırma Web sitesi, Çözüm Gezgini'nde simgesini izleyen tıklatarak başlatın.

![](mvc-music-store-part-7/_static/image2.png)

Bu yapılandırma Web sitesini başlatır. Giriş ekranı Güvenlik sekmesinde, ardından ekranın Center'da "rolleri etkinleştir" bağlantısını tıklatın.

![](mvc-music-store-part-7/_static/image3.png)

"Rol oluşturma ve yönetme" bağlantısını tıklatın.

![](mvc-music-store-part-7/_static/image4.png)

"Yönetici" rolü adı girin ve Rol Ekle düğmesine basın.

![](mvc-music-store-part-7/_static/image5.png)

Geri düğmesini tıklatın, ardından sol tarafındaki oluşturma kullanıcı bağlantısına tıklayın.

![](mvc-music-store-part-7/_static/image6.png)

Aşağıdaki bilgileri kullanarak soldaki kullanıcı bilgi alanları doldurun:

| **Alan** | **Değer** |
| --- | --- |
| **Kullanıcı adı** | Yönetici |
| **Parola** | password123! |
| **Parolayı onaylayın** | password123! |
| **E-posta** | (herhangi bir e-posta adresi çalışır) |
| **Güvenlik sorusu** | (ne olursa olsun, ister) |
| **Güvenlik yanıtı** | (ne olursa olsun, ister) |

*Not: İstediğiniz herhangi bir parola Elbette kullanabilirsiniz. Varsayılan parola güvenlik ayarları 7 karakterden uzun olduğundan ve bir tane alfasayısal olmayan karakter içeren bir parola gerektirir.*

Bu kullanıcı için yönetici rolünü seçin ve kullanıcı Oluştur düğmesine tıklayın.

![](mvc-music-store-part-7/_static/image7.png)

Bu noktada, kullanıcının başarıyla oluşturulduğunu belirten bir ileti görürsünüz.

![](mvc-music-store-part-7/_static/image8.png)

Bir tarayıcı penceresi artık kapatabilirsiniz.

## <a name="role-based-authorization"></a>Rol tabanlı yetkilendirme

Şimdi biz kullanıcı sınıfı içinde herhangi bir denetleyici işlem erişmek için Yönetici rolü olmalıdır belirterek [Authorize] özniteliğini kullanarak StoreManagerController erişimi kısıtlayabilirsiniz.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Not: [Authorize] özniteliği ve denetleyici sınıf düzeyinde aynı zamanda belirli eylem yöntemleri yerleştirilebilir.*

Şimdi /StoreManager için gözatma bir oturum açma iletişim kutusunu açar:

![](mvc-music-store-part-7/_static/image9.png)

Bizim yeni yönetici hesabı kullanarak oturum açtıktan sonra biz önce albümü düzenle ekran çalışmaya devam edebilirsiniz.

>[!div class="step-by-step"]
[Önceki](mvc-music-store-part-6.md)
[sonraki](mvc-music-store-part-8.md)
