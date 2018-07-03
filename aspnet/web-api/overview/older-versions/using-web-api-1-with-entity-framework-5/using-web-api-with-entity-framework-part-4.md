---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: '4. Bölüm: yönetici görünümü ekleme | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 599f684ba200821d7fcd33819937c5a5b5a29ce8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371057"
---
<a name="part-4-adding-an-admin-view"></a>4. Bölüm: yönetici görünümü ekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Yönetici görünümü ekleme

Şimdi biz için istemci tarafı açın ve yönetim denetleyicisi veri tüketebilen bir sayfa ekleyin. Sayfayı oluşturmak, düzenlemek veya AJAX isteklerini denetleyiciye göndererek ürünleri, silme imkan tanıyacak.

Çözüm Gezgini'nde denetleyicileri klasörünü genişletin ve HomeController.cs adlı dosyayı açın. Bu dosya, MVC denetleyicisi içerir. Adlı bir yöntem ekleyin `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** yöntemi, web API'si için URI oluşturur ve bu görünüm paketini daha sonra kullanmak üzere depolarız.

Ardından, içinde metin imleci konumlandırma `Admin` eylem yöntemi, sonra sağ tıklatın ve seçin **Görünüm Ekle**. Bu getirir **Görünüm Ekle** iletişim.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

İçinde **Görünüm Ekle** iletişim kutusunda, "Yönetici" Görünüm adı. Etiketli onay kutusunu seçin **kesin türü belirtilmiş görünüm oluşturmak**. Altında **Model sınıfı**, "Ürün (ProductStore.Models)" seçin. Diğer seçenekleri varsayılan değerlerinde bırakın.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Tıklayarak **Ekle** Admin.cshtml görünümler/giriş altında adlı bir dosya ekler. Bu dosyayı açın ve aşağıdaki HTML'yi ekleyin. Bu HTML sayfası yapısını tanımlar, ancak hiçbir işlevsellik yukarı henüz hazırlanmıştır.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Yönetici sayfasına bağlantı oluştur

Çözüm Gezgini'nde görünümler klasörünü genişletin ve sonra paylaşılan klasörünü genişletin. Adlı dosyayı açın \_Layout.cshtml. Bulun **ul** öğesi kimliği = "menüsünde" ve yönetici görünümü için bir eylem bağlantısı:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Örnek projesinde dizenin "Logonuz buraya gelir" gibi birkaç başka yüzeysel değişiklikler, yaptım. Bu, uygulamanın işlevselliğini etkilemez. Projenizi indirin ve dosyayı Karşılaştır.


Uygulamayı çalıştırmak ve giriş sayfasının üst kısmında görünür "Yönetici" bağlantısına tıklayın. Yönetici sayfasına aşağıdaki gibi görünmelidir:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Sağdaki şimdi, sayfayı hiçbir şey yapmıyor. Sonraki bölümde, bir dinamik kullanıcı Arabirimi oluşturmak için Knockout.js kullanacağız.

## <a name="add-authorization"></a>Yetkilendirme Ekle

Yönetici sayfasına sitesini ziyaret eden herkes şu anda erişilebilir. Yöneticilerin kısıtlamak için bu değiştirelim.

Bir "Yönetici" rolü ve bir yönetici kullanıcı ekleyerek başlayın. Çözüm Gezgini'nde filtreleri klasörünü genişletin ve InitializeSimpleMembershipAttribute.cs adlı dosyayı açın. Bulun `SimpleMembershipInitializer` Oluşturucusu. Çağrısından sonra **WebSecurity.InitializeDatabaseConnection**, aşağıdaki kodu ekleyin:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Bu, "Yönetici" rolünü ekleyin ve bir kullanıcı rolü oluşturmak için quick-and-dirty bir yoludur.

Çözüm Gezgini'nde denetleyicileri klasörünü genişletin ve HomeController.cs dosyasını açın. Ekleme **Authorize** özniteliğini `Admin` yöntemi.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

AdminController.cs dosyasını açın ve eklemek **Authorize** özniteliği için tüm `AdminController` sınıfı.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC ve Web API'si hem de tanımlama **Authorize** farklı ad alanlarında öznitelikleri. MVC kullanır **System.Web.Mvc.AuthorizeAttribute**, Web API kullanırken **System.Web.Http.AuthorizeAttribute**.


Artık yalnızca Yöneticiler, yönetici sayfasına görüntüleyebilirsiniz. Ayrıca, yönetici denetleyiciye bir HTTP isteği gönderirseniz, istek bir kimlik doğrulama tanımlama bilgisi içermesi gerekir. Aksi durumda, sunucu, bir HTTP 401 (yetkisiz) yanıt gönderir. Bir GET isteği göndererek bu Fiddler'da görebilirsiniz `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-3.md)
> [İleri](using-web-api-with-entity-framework-part-5.md)
