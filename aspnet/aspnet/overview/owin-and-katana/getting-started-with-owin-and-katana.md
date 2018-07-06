---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: OWIN ve Katana ile çalışmaya başlama | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 58a3d234867821d5e23cce2f01e105dfab88ac33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826672"
---
<a name="getting-started-with-owin-and-katana"></a>OWIN ve Katana ile çalışmaya başlama
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[.NET (OWIN) için açık Web arabirimi](http://owin.org/) .NET web sunucuları ve web uygulaması arasında bir Özet tanımlar. Uygulamayı web sunucusundan ayrıştırarak OWIN ara yazılımı için .NET web geliştirme oluşturmak kolaylaştırır. Ayrıca, OWIN diğer Konaklara bağlantı noktası web uygulamaları kolaylaştırır&#8212;Örneğin, bir Windows hizmeti veya başka bir işlem kendi kendine barındırma.

OWIN uygulaması değil bir topluluk ait belirtimi ' dir. Katana proje, Microsoft tarafından geliştirilen bir açık kaynak OWIN bileşenleri kümesidir. OWIN ve Katana hem genel bir bakış için bkz. [bir genel bakış, Project Katana'ya](an-overview-of-project-katana.md). Bu makalede, ı kullanmaya başlamak için kod sağ atlayacaktır.

Bu öğreticide [Visual Studio 2013 Sürüm Adayı](https://go.microsoft.com/fwlink/?LinkId=306566), ancak Visual Studio 2012'de kullanabilirsiniz. Birkaç adımın ı aşağıda unutmayın Visual Studio 2012'de farklıdır.

## <a name="host-owin-in-iis"></a>IIS'de OWIN barındırma

Bu bölümde, biz IIS'de OWIN barındırılacak. Bu seçenek IIS'in olgun özellik kümesi ile birlikte bir OWIN Ardışık düzenin composability ve esneklik sağlar. Bu seçeneği kullanarak ASP.NET isteği işlem hattında OWIN uygulaması çalışır.

İlk olarak, yeni bir ASP.NET Web uygulaması projesi oluşturun. (Visual Studio 2012'de ASP.NET boş Web uygulaması proje türünü kullanın.)

![](getting-started-with-owin-and-katana/_static/image1.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>NuGet paketleri Ekle

Ardından, gerekli NuGet paketlerini ekleyin. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Başlangıç sınıfı ekleme

Ardından, OWIN başlangıç sınıfı ekleyin. Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle**, ardından **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Owın başlangıç sınıfı**. Başlangıç sınıfı yapılandırma hakkında daha fazla bilgi için bkz. [OWIN başlangıç sınıfı algılama](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Aşağıdaki kodu ekleyin `Startup1.Configuration` yöntemi:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Bu kod basit bir ara yazılım alan bir işlev uygulanan OWIN ardışık düzenine ekler. bir **Microsoft.Owin.IOwinContext** örneği. Sunucu HTTP isteği aldığında, OWIN ardışık düzenine bir ara yazılım çağırır. Ara yazılım, içerik türü yanıtı için ayarlar ve yanıt gövdesi olarak yazar.

> [!NOTE]
> OWIN başlangıç sınıfı şablon, Visual Studio 2013'te kullanılabilir. Visual Studio 2012 kullanıyorsanız, yalnızca adında yeni bir boş Sınıf Ekle `Startup1`, aşağıdaki kodu yapıştırın:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Uygulamayı çalıştırın

Hata ayıklamayı başlatmak için F5 tuşuna basın. Visual Studio için bir tarayıcı penceresi açılır `http://localhost:*port*/`. Sayfa aşağıdaki gibi görünmelidir:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Bir konsol uygulamasında OWIN barındırma

Bu uygulama, özel bir işlemde kendi kendine barındırma için IIS barındırma dönüştürmek kolay bir işlemdir. IIS barındırma ile IIS hem HTTP sunucusunu ve hizmetini barındıran işlemi olarak görev yapar. Kendi kendine barındırma ile uygulamanızı bir işlem oluşturur ve kullanır **HttpListener** HTTP sunucusuyla sınıfı.

Visual Studio'da yeni bir konsol uygulaması oluşturun. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Ekleme bir `Startup1` projeye bu öğreticinin 1 bölümünden sınıfı. Bu sınıf değiştirmeniz gerekmez.

Uygulamanın uygulama `Main` yöntemini aşağıdaki şekilde.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Konsol uygulamasını çalıştırdığınızda, sunucuyu başlatır dinleme `http://localhost:9000`. Bir web tarayıcısında bu adrese gidin, "Hello world" sayfası görürsünüz.

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>OWIN tanılama Ekle

İşlenmeyen özel durumları yakalar ve hata ayrıntılarını içeren bir HTML sayfası görüntüleyen bir ara yazılım Microsoft.Owin.Diagnostics paket içerir. Bu sayfa işlevlerini benzer şekilde adlandırılır ASP.NET hata sayfası "[Ölüm sarı ekran](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). YSOD gibi geliştirme sırasında Katana hata sayfası yararlıdır, ancak üretim modunda devre dışı bırakmak için iyi bir uygulamadır.

Tanılama paketini projenize yüklemek için Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:

`install-package Microsoft.Owin.Diagnostics –Pre`

Kodda değişiklik, `Startup1.Configuration` yöntemini aşağıdaki şekilde:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Visual Studio özel durumla ilgili kesmemesi böylece artık uygulamayı hata ayıklama olmadan çalıştırmak için CTRL + F5'nı kullanın. Gittikten kadar uygulama önceki gibi davranır `http://localhost/fail`, bu noktada uygulama özel durum oluşturur. Hata sayfası ara yazılımını özel durumu yakalar ve hata hakkında bilgi içeren bir HTML sayfası görüntüler. Yığın, sorgu dizesi, tanımlama bilgileri, istek üst bilgisi ve OWIN ortam değişkenleri görmek için sekmeler tıklayabilirsiniz.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Sonraki Adımlar

- [OWIN Başlangıç Sınıfı Algılama](owin-startup-class-detection.md)
- [Barındırılan ASP.NET Web API'si için OWIN kullanın](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [SignalR barındırma için OWIN kullanın](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
