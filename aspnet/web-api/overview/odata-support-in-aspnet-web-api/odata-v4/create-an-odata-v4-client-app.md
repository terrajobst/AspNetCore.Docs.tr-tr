---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: OData v4 istemci uygulaması (C#) oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1e621988b43b4f012f113d7a52250879da56bb1c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818682"
---
<a name="create-an-odata-v4-client-app-c"></a>OData v4 istemci uygulaması (C#) oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Önceki öğreticide, temel CRUD işlemleri destekleyen bir OData hizmeti oluşturuldu. Artık bir istemci hizmeti için oluşturalım.

Yeni bir Visual Studio örneği başlatın ve yeni bir konsol uygulama projesi oluşturun. İçinde **yeni proje** iletişim kutusunda **yüklü** &gt; **şablonları** &gt; **Visual C#** &gt; **Windows Masaüstü**seçip **konsol uygulaması** şablonu. Projeyi adlandırın &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Ayrıca, OData hizmeti içeren aynı Visual Studio çözümünü konsol uygulaması ekleyebilirsiniz.


## <a name="install-the-odata-client-code-generator"></a>OData istemci kodu oluşturucuyu yükleme

Gelen **Araçları** menüsünde **Uzantılar ve güncelleştirmeler**. Seçin **çevrimiçi** &gt; **Visual Studio Galerisi**. Arama kutusuna arama &quot;OData istemci kodu Oluşturucu&quot;. Tıklayın **indirme** VSIX'i yüklemek. Visual Studio'yu yeniden başlatmanız istenebilir.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>OData hizmeti yerel olarak çalıştırma

Visual Studio'dan ProductService projeyi çalıştırın. Varsayılan olarak, Visual Studio uygulama kökü için bir tarayıcı başlatır. URI dikkat edin. Bu sonraki adımda gerekir. Uygulamasını çalışır durumda bırakın.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Aynı çözüm içinde her iki proje yerleştirdiğinizde ProductService projeyi hata ayıklama olmadan çalıştırmak emin olun. Sonraki adımda, konsol uygulama projesi değiştirme sırasında çalışan hizmeti tutmanız gerekir.


## <a name="generate-the-service-proxy"></a>Hizmet proxy'si oluştur

Hizmet proxy'si OData hizmetine erişim yöntemleri tanımlayan bir .NET sınıfıdır. Ara sunucu HTTP isteklerinin yöntemi çağrılarını çevirir. Proxy sınıfı çalıştırarak oluşturacağınız bir [T4 şablonu](https://msdn.microsoft.com/library/bb126445.aspx).

Projeye sağ tıklayın. Seçin **ekleme** &gt; **yeni öğe**.

![](create-an-odata-v4-client-app/_static/image5.png)

İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# öğeleri** &gt; **kod** &gt; **OData istemcisi**. Şablon adı &quot;ProductClient.tt&quot;. Tıklayın **Ekle** aracılığıyla güvenlik uyarısı tıklayın.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

Bu noktada, siz görmezden gelmenizde bir hata alırsınız. Visual Studio, şablonu otomatik olarak çalışır, ancak bazı yapılandırma ayarları şablonun gerekir ilk.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

ProductClient.odata.config dosyasını açın. İçinde `Parameter` öğesini ProductService projesi (önceki adımda) URI'SİNDEN yapıştırın. Örneğin:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Şablonu yeniden çalıştırın. Çözüm Gezgini'nde ProductClient.tt dosyasını sağ tıklatın ve seçin **özel aracı Çalıştır**.

Şablon proxy tanımlayan ProductClient.cs adlı bir kod dosyası oluşturur. OData uç noktasını değiştirirseniz, uygulamanızı geliştirirken, şablonun proxy güncelleştirmeyi yeniden çalıştırın.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>OData hizmeti çağırmak amacıyla hizmet proxy'si kullanın

Program.cs dosyasını açın ve ortak kod aşağıdakiyle değiştirin.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Değiştirin *serviceUri* hizmet URI'si ile daha önce.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Uygulamayı çalıştırdığınızda, aşağıdaki çıkışı oluşturmalıdır:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
