---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: "Azure Service Bus ile SignalR genişletme (SignalR 1.x) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Azure Service Bus ile SignalR genişletme (SignalR 1.x)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartı kullanarak rolü bir SignalR uygulamasına dağıtırsınız.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Önkoşullar:

- Bir Windows Azure hesabı.
- [Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Hizmet veri yolu devre kartı ayrıca ile uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), sürüm 1.1. Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.

## <a name="pricing"></a>Fiyatlandırma

Hizmet veri yolu devre kartı konuları iletileri göndermek için kullanır. En son fiyatlandırma bilgileri için bkz: [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Bu yazma zaman $1'den için aylık 1.000.000 iletileri gönderebilir. Devre kartına her bir SignalR hub yönteminin çağrılması için hizmet veri yolu ileti gönderir. Bağlantılar, bağlantısının kesilmesi, birleşme veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır. Çoğu uygulamada ileti trafiği çoğunluğu hub yöntemi çağrılarına olacaktır.

## <a name="overview"></a>Genel Bakış

Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.

1. Yeni bir hizmet veri yolu ad alanı oluşturmak için Windows Azure Portalı'nı kullanın.
2. Bu NuGet paketleri uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Her uygulama için "Uygulamanızınadı" için farklı bir değer seçin. Aynı değeri birden çok uygulama arasında kullanmayın.

## <a name="create-the-azure-services"></a>Azure hizmetleri oluşturma

Bölümünde açıklandığı gibi bir bulut hizmeti Oluştur [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Bölümündeki adımları "nasıl yapılır: hızlı Oluştur kullanarak bir bulut hizmeti oluşturma". Bu öğreticide, bir sertifikayı karşıya yüklemek gerekmez.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Bölümünde açıklandığı gibi yeni bir hizmet veri yolu ad alanı oluşturma [nasıl kullanım Service Bus konuları/abonelikleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). "Hizmet Namespace oluşturma" bölümündeki adımları izleyin.

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Bulut hizmeti ve Service Bus ad alanı için aynı bölgede seçtiğinizden emin olun.


## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio'yu başlatın. Gelen **dosya** menüsünde tıklatın **yeni proje**.

İçinde **yeni proje** iletişim kutusunda, genişletin **Visual C#**. Altında **yüklü şablonlar**seçin **bulut** ve ardından **Windows Azure bulut hizmeti**. Varsayılan .NET Framework 4.5 tutun. ChatService uygulama adı ve'ı tıklatın **Tamam**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, select ASP.NET MVC 4 Web rolü. Sağ ok düğmesine tıklayın (**&gt;**) çözümünüze rolü eklemek için.

Fare yeni rol, bu nedenle vurgulu kalem simgesi görünür. Rolü yeniden adlandırmak için bu simgeyi tıklatın. "SignalRChat" rol adını verin ve tıklayın **Tamam**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

İçinde **yeni ASP.NET MVC 4 proje** seçin **Internet uygulama**. **Tamam**'ı tıklatın. Proje Sihirbazı iki proje oluşturur:

- ChatService: Bu proje Windows Azure uygulamasıdır. Azure rol ve diğer yapılandırma seçeneklerini tanımlar.
- SignalRChat: Bu, ASP.NET MVC 4 Proje projesidir.

## <a name="create-the-signalr-chat-application"></a>SignalR sohbet uygulaması oluşturma

Sohbet uygulaması oluşturmak için öğreticide adımları [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).

Gerekli kitaplıkları yükleme için NuGet kullanın. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Kullanım `-ProjectName` Microsoft Azure projesi yerine ASP.NET MVC projesinin paketleri yüklemek için seçeneği.

## <a name="configure-the-backplane"></a>Devre kartına yapılandırın

Uygulamanızın Global.asax dosyasında aşağıdaki kodu ekleyin:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Artık service bus bağlantı dizenizi almanız gerekir. Azure portalında oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Bağlantı dizesini Pano'ya kopyalayın ve yapıştırın *connectionString* değişkeni.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Azure'a dağıtma

Çözüm Gezgini'nde genişletin **rolleri** ChatService proje klasöründen.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

SignalRChat role sağ tıklayın ve seçin **özellikleri**. Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin. De VM boyutu ayarlayabilirsiniz **ek küçük**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Değişiklikleri kaydedin.

Çözüm Gezgini'nde ChatService projesine sağ tıklayın. Seçin **yayımlama**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Bu, ilk Windows Azure zaman yayımlama ise, kimlik bilgilerinizi indirmeniz gerekir. İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'i tıklatın. Bu Windows Azure portalında oturum açın ve yayımlama ayarları dosyasını indirme isteyip istemediğinizi sorar.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Tıklatın **alma** ve indirdiğiniz yayımlama ayarları dosyasını seçin.

**İleri**'ye tıklayın. İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bulut hizmeti seçin.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Tıklatın **yayımlama**. Uygulamayı dağıtmak ve sanal makineleri başlatmak için birkaç dakika sürebilir.

Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerinin bir Service Bus konu kullanarak Azure Service Bus iletişim kurar. Birden çok aboneye sağlayan bir ileti sırası konudur.

Devre kartına, konu ve abonelik otomatik olarak oluşturur. İleti etkinliği ve abonelikleri görmek için Azure portalını açın, hizmet veri yolu ad alanını seçin ve "Konularda"'i tıklatın.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Anlaması panosunda göstermeyi ileti etkinliği için birkaç dakika sürer.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR konu yaşam süresini yönetir. Uygulamanızı dağıttınız sürece, el ile konuları silmek veya konuyla ilgili ayarları değiştirmek denemeyin.
