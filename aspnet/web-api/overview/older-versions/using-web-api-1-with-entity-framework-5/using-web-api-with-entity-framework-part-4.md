---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: "Bölüm 4: bir yönetici görünümü ekleme | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2960eee37201655a9e4632bf0196ba18a0e2e82a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-4-adding-an-admin-view"></a>Bölüm 4: bir yönetici görünümü ekleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Bir yönetim görünümü ekleme

Şimdi biz istemci tarafı açın ve yönetim denetleyicisi verilerden tüketebileceği bir sayfa ekleyin. Sayfayı oluşturmak, düzenlemek veya denetleyicisine AJAX isteği göndererek ürünleri silmek kullanıcılara izin verir.

Çözüm Gezgini'nde, denetleyicileri klasörünü genişletin ve HomeController.cs adlı dosyayı açın. Bu dosya, MVC denetleyicisi içerir. Adlı bir yöntem ekleyin `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** yöntemi, web API'si için URI oluşturur ve bu görünüm paketi için daha sonra depolarız.

Ardından, metin imleci içinde getirin `Admin` eylem yöntemi, sonra sağ tıklatın ve seçin **Görünüm Ekle**. Bu getirir **Görünüm Ekle** iletişim.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

İçinde **Görünüm Ekle** iletişim kutusunda, adı "Yönetici" görüntüleme. Etiketli onay kutusunu seçin **kesin türü belirtilmiş görünüm oluşturmak**. Altında **Model sınıfı**, "Ürün (ProductStore.Models)" seçin. Diğer tüm seçenekleri varsayılan değerlerine bırakın.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Tıklatarak **Ekle** görünümler/giriş altında Admin.cshtml adlı bir dosyayı ekler. Bu dosyayı açın ve aşağıdaki HTML ekleyin. Bu HTML sayfası yapısını tanımlar, ancak hiçbir işlevsellik yukarı henüz kablolu ise.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Yönetim sayfasına bir bağlantı oluştur

Çözüm Gezgini'nde, görünümler klasörünü genişletin ve ardından paylaşılan klasörünü genişletin. Adlı dosyayı açın \_Layout.cshtml. Bulun **ul** kimlikli öğe = "menüsünden" ve yönetim görünümü için bir eylem bağlantısı:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Örnek projesinde ediyorum "Buraya logonuz konacak" dize değiştirme gibi birkaç diğer yüzeysel, yapılan değişiklikler. Bu uygulamanın işlevselliğini etkilemez. Projenizi indirin ve dosyaları karşılaştırın.


Uygulamayı çalıştırın ve giriş sayfasının en üstünde görünür "Yönetici" bağlantısını tıklatın. Yönetim sayfasında, aşağıdaki gibi görünmelidir:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Sağ şimdi, sayfa hiçbir şey yapmaz. Sonraki bölümde, dinamik kullanıcı arabirimini oluşturmak için Knockout.js kullanacağız.

## <a name="add-authorization"></a>Yetkilendirme ekleme

Yönetim sayfası siteyi ziyaret eden herkes için şu anda erişilebilir. Bu yöneticilere erişimi kısıtlamak için değiştirelim.

Bir "Yönetici" rolü ve bir yönetici kullanıcı ekleyerek başlayın. Çözüm Gezgini'nde, filtreleri klasörünü genişletin ve InitializeSimpleMembershipAttribute.cs adlı dosyayı açın. Bulun `SimpleMembershipInitializer` Oluşturucusu. Çağrısından sonra **WebSecurity.InitializeDatabaseConnection**, aşağıdaki kodu ekleyin:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Bu, "Yönetici" rolü eklemek ve bir kullanıcı rolü oluşturmak için quick-and-dirty bir yoludur.

Çözüm Gezgini'nde denetleyicileri klasörünü genişletin ve HomeController.cs dosyasını açın. Ekleme **Authorize** özniteliğini `Admin` yöntemi.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

AdminController.cs dosyasını açın ve eklemek **Authorize** özniteliği için tüm `AdminController` sınıfı.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC ve Web API her ikisi de tanımlamak **Authorize** farklı ad alanlarında öznitelikleri. MVC kullanır **System.Web.Mvc.AuthorizeAttribute**, Web API kullanırken **System.Web.Http.AuthorizeAttribute**.


Artık yalnızca yöneticiler yönetim sayfasında görüntüleyebilirsiniz. Ayrıca, yönetim denetleyicisi için bir HTTP isteği gönderirseniz, istek bir kimlik doğrulama tanımlama bilgisini içermelidir. Aksi durumda, sunucu, bir HTTP 401 (yetkisiz) yanıt gönderir. Bir GET isteği göndererek bu Fiddler'da görebilirsiniz `http://localhost:*port*/api/admin`.

>[!div class="step-by-step"]
[Önceki](using-web-api-with-entity-framework-part-3.md)
[sonraki](using-web-api-with-entity-framework-part-5.md)
