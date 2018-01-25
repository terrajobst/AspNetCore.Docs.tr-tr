---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "OData v4 istemci uygulaması (C#) oluşturma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>OData v4 istemci uygulaması (C#) oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Önceki öğreticide, temel CRUD işlemleri destekleyen bir OData hizmet oluşturuldu. Şimdi bir istemci hizmeti için oluşturalım.

Visual Studio yeni bir örneğini Başlat ve yeni bir konsol uygulama projesi oluşturun. İçinde **yeni proje** iletişim kutusunda **yüklü** &gt; **şablonları** &gt; **Visual C#** &gt; **Windows Masaüstü**seçip **konsol uygulaması** şablonu. Proje adı &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> OData hizmeti içeren aynı Visual Studio çözümü konsol uygulaması de ekleyebilirsiniz.


## <a name="install-the-odata-client-code-generator"></a>OData istemci kod oluşturucuyu yükleme

Gelen **Araçları** menüsünde, select **Uzantılar ve güncelleştirmeler**. Seçin **çevrimiçi** &gt; **Visual Studio Galerisi**. Arama kutusuna arama &quot;OData istemci Kod Oluşturucu&quot;. Tıklatın **karşıdan** VSIX yüklemek için. Visual Studio yeniden başlatmanız istenebilir.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>OData hizmeti yerel olarak çalıştırma

ProductService projesini Visual Studio'dan çalıştırın. Varsayılan olarak, Visual Studio uygulama kökü tarayıcıya başlatır. URI unutmayın; Bu sonraki adımda gerekir. Çalışan uygulama bırakın.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Aynı çözüm içinde her iki proje yerleştirirseniz ProductService projenin hata ayıklama olmadan çalıştırma emin olun. Sonraki adımda, konsol uygulama projesi değiştirme sırada çalışan hizmeti tutmanız gerekir.


## <a name="generate-the-service-proxy"></a>Hizmet proxy'si oluştur

Hizmet proxy'si OData hizmetine erişmek için yöntemler tanımlayan bir .NET sınıfıdır. Proxy HTTP istek yöntemi çağrılarını çevirir. Proxy sınıfı çalıştırarak oluşturacak bir [T4 şablon](https://msdn.microsoft.com/library/bb126445.aspx).

Projeye sağ tıklayın. Seçin **ekleme** &gt; **yeni öğe**.

![](create-an-odata-v4-client-app/_static/image5.png)

İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# öğeleri** &gt; **kod** &gt; **OData istemci**. Şablon adı &quot;ProductClient.tt&quot;. Tıklatın **Ekle** ve güvenlik uyarısı aracılığıyla'ı tıklatın.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

Bu noktada, göz ardı edebilirsiniz bir hata iletisi alırsınız. Visual Studio şablon otomatik olarak çalışır, ancak bazı yapılandırma ayarları şablon gerekiyor ilk.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

ProductClient.odata.config dosyasını açın. İçinde `Parameter` öğesi, URI (önceki adımda) ProductService projeden Yapıştır. Örneğin:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Şablonu yeniden çalıştırın. Çözüm Gezgini'nde, ProductClient.tt dosyasını sağ tıklatın ve seçin **çalıştırmak özel araç**.

Şablon proxy tanımlar ProductClient.cs adlı bir kod dosyası oluşturur. OData uç noktasını değiştirirseniz, uygulamanızı geliştirirken, şablonu proxy güncelleştirmeyi yeniden çalıştırın.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>OData hizmetini çağırmak için hizmet proxy'si kullanın

Program.cs dosyasını açın ve demirbaş kod aşağıdakiyle değiştirin.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Değerini *serviceUri* daha önce gelen hizmet URI'si ile.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Uygulamayı çalıştırdığınızda, aşağıdaki çıkış:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
