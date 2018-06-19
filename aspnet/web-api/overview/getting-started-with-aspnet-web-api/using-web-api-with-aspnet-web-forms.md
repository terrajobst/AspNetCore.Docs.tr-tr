---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web Forms ile Web API kullanma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26575856"
---
<a name="using-web-api-with-aspnet-web-forms"></a>ASP.NET Web Forms ile Web API kullanma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

ASP.NET Web API, ASP.NET MVC ile paketlenmiştir rağmen geleneksel bir ASP.NET Web Forms uygulamasına Web API ekleme kolaydır. Bu öğretici adımlarda size yol gösterir.

## <a name="overview"></a>Genel Bakış

Bir Web Forms uygulamasında Web API kullanmak için iki ana adım vardır:

- Türetilen bir Web API denetleyicisi eklemek **ApiController** sınıfı.
- Bir rota tablosuna eklemek **uygulama\_Başlat** yöntemi.

## <a name="create-a-web-forms-project"></a>Web Forms projesi oluşturma

Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET Web Forms uygulaması**. Proje için bir ad girin ve tıklayın **Tamam**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Model ve denetleyici oluşturma

Bu öğretici olarak aynı model ve denetleyici sınıfları kullanır [Başlarken](tutorial-your-first-web-api.md) Öğreticisi.

İlk olarak, bir model sınıfı ekleyin. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **sınıfı Ekle**. Ürün sınıf adını ve aşağıdaki uygulama ekleyin:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Ardından, Web API denetleyicisi proje, A ekleyin. *denetleyicisi* için Web API HTTP isteklerini işler nesnesidir.

İçinde **Çözüm Gezgini**, projeye sağ tıklayın. Seçin **Yeni Öğe Ekle**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Altında **yüklü şablonlar**, genişletin **Visual C#** seçip **Web**. Şablonları listesinden seçip **Web API denetleyicisi sınıfı**. "ProductsController" Denetleyici adı ve'ı tıklatın **Ekle**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Yeni Öğe Ekle** Sihirbazı ProductsController.cs adlı bir dosya oluşturur. Sihirbaz dahil yöntemlerini silin ve aşağıdaki yöntemleri ekleyin:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Bu denetleyici kodda hakkında daha fazla bilgi için bkz: [Başlarken](tutorial-your-first-web-api.md) Öğreticisi.

## <a name="add-routing-information"></a>Yönlendirme bilgileri

Ardından, bir URI yolu böylece ekleyeceğiz bu URI biçiminde &quot;/api/ürünler/&quot; denetleyiciye yönlendirilir.

İçinde **Çözüm Gezgini**, Global.asax Global.asax.cs arka plan kodu dosyayı açmak için çift tıklayın. Aşağıdakileri ekleyin **kullanarak** deyimi.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Ardından aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Yönlendirme tabloları hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>İstemci tarafı AJAX ekleme

Tüm web istemcileri erişebilir API'si oluşturmak için gereken budur. Şimdi, API'yi çağırmak için jQuery kullanan bir HTML sayfası ekleyelim.

Ana sayfanızın emin olun (örneğin, *Site.Master*) içeren bir `ContentPlaceHolder` ile `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Default.aspx dosyasını açın. Ana içerik bölümü Demirbaş metni gösterildiği gibi değiştirin:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Ardından, jQuery kaynak dosyasına bir başvuru ekleyin `HeaderContent` bölümü:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Not: Kolayca komut dosyası başvurusunu dosyasından bırakarak ekleyebilirsiniz **Çözüm Gezgini** Kod Düzenleyicisi penceresine.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Aşağıdaki betik bloğu jQuery komut dosyası etiketinin ekleyin:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Belge yüklendiğinde, bu komut dosyası için bir AJAX isteği yapar &quot;API/ürünleri&quot;. İstek JSON biçiminde ürünlerin listesini döndürür. Betik ürün bilgilerini HTML tabloya ekler.

Uygulamayı çalıştırdığınızda, aşağıdaki gibi görünmelidir:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
