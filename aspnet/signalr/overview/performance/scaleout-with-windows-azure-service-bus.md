---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: SignalR genişletme Azure Service Bus ile | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri bu konu Visual Studio 2013 .NET 4.5 SignalR sürümünde bu konuda Bu konu için SignalR 1.x sürümü 2 önceki sürümlerinde kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e6d9e4e6ba2040aa2c6e453aacf0ddca38c4a1a9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "32741546"
---
<a name="signalr-scaleout-with-azure-service-bus"></a>Azure Service Bus ile SignalR genişletme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartı kullanarak rolü bir SignalR uygulamasına dağıtırsınız. (Service Bus devre kartı ile de kullanabileceğiniz [web uygulamaları Azure App Service'te](https://docs.microsoft.com/azure/app-service-web/).)

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Önkoşullar:

- Bir Windows Azure hesabı.
- [Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 veya 2013.

Hizmet veri yolu devre kartı ayrıca ile uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), sürüm 1.1. Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.

## <a name="pricing"></a>Fiyatlandırma

Hizmet veri yolu devre kartı konuları iletileri göndermek için kullanır. En son fiyatlandırma bilgileri için bkz: [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Bu yazma zaman $1'den için aylık 1.000.000 iletileri gönderebilir. Devre kartına her bir SignalR hub yönteminin çağrılması için hizmet veri yolu ileti gönderir. Bağlantılar, bağlantısının kesilmesi, birleşme veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır. Çoğu uygulamada ileti trafiği çoğunluğu hub yöntemi çağrılarına olacaktır.

## <a name="overview"></a>Genel Bakış

Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.

1. Yeni bir hizmet veri yolu ad alanı oluşturmak için Windows Azure Portalı'nı kullanın.
2. Bu NuGet paketleri uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) veya [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için haline için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Bu kod için varsayılan değerlerle devre kartı yapılandırır [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) ve [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Bu değerleri değiştirme hakkında daha fazla bilgi için bkz: [SignalR performans: genişletme ölçümleri](signalr-performance.md#scaleout_metrics).

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

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda, ASP.NET Web rolü seçin. Sağ ok düğmesine tıklayın (**&gt;**) çözümünüze rolü eklemek için.

Fare yeni rol, bu nedenle vurgulu kalem simgesi görünür. Rolü yeniden adlandırmak için bu simgeyi tıklatın. "SignalRChat" rol adını verin ve tıklayın **Tamam**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC**, Tamam'ı tıklatın.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Proje Sihirbazı iki proje oluşturur:

- ChatService: Bu proje Windows Azure uygulamasıdır. Azure rol ve diğer yapılandırma seçeneklerini tanımlar.
- SignalRChat: Bu, ASP.NET MVC 5 projeniz projesidir.

## <a name="create-the-signalr-chat-application"></a>SignalR sohbet uygulaması oluşturma

Sohbet uygulaması oluşturmak için öğreticide adımları [SignalR ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Gerekli kitaplıkları yükleme için NuGet kullanın. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Kullanım `-ProjectName` Microsoft Azure projesi yerine ASP.NET MVC projesinin paketleri yüklemek için seçeneği.

## <a name="configure-the-backplane"></a>Devre kartına yapılandırın

Uygulamanızın haline dosyasına aşağıdaki kodu ekleyin:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Artık service bus bağlantı dizenizi almanız gerekir. Azure portalında oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Bağlantı dizesini Pano'ya kopyalayın ve yapıştırın *connectionString* değişkeni.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Çözüm Gezgini'nde genişletin **rolleri** ChatService proje klasöründen.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

SignalRChat role sağ tıklayın ve seçin **özellikleri**. Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin. De VM boyutu ayarlayabilirsiniz **ek küçük**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Değişiklikleri kaydedin.

Çözüm Gezgini'nde ChatService projesine sağ tıklayın. Seçin **yayımlama**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Bu, ilk Windows Azure zaman yayımlama ise, kimlik bilgilerinizi indirmeniz gerekir. İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'i tıklatın. Bu Windows Azure portalında oturum açın ve yayımlama ayarları dosyasını indirme isteyip istemediğinizi sorar.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Tıklatın **alma** ve indirdiğiniz yayımlama ayarları dosyasını seçin.

**İleri**'ye tıklayın. İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bulut hizmeti seçin.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Tıklatın **yayımlama**. Uygulamayı dağıtmak ve sanal makineleri başlatmak için birkaç dakika sürebilir.

Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerinin bir Service Bus konu kullanarak Azure Service Bus iletişim kurar. Birden çok aboneye sağlayan bir ileti sırası konudur.

Devre kartına, konu ve abonelik otomatik olarak oluşturur. İleti etkinliği ve abonelikleri görmek için Azure portalını açın, hizmet veri yolu ad alanını seçin ve "Konularda"'i tıklatın.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

Anlaması panosunda göstermeyi ileti etkinliği için birkaç dakika sürer.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

SignalR konu yaşam süresini yönetir. Uygulamanızı dağıttınız sürece, el ile konuları silmek veya konuyla ilgili ayarları değiştirmek denemeyin.

## <a name="troubleshooting"></a>Sorun giderme

**System.InvalidOperationException "Yalnızca desteklenen IsolationLevel 'IsolationLevel.Serializable' değil."**

Bir işlem için işlem düzeyi bir şeye dışında ayarlandıysa, bu hata oluşabilir `Serializable`. Diğer işlem düzeyleriyle hiçbir işlem gerçekleştiriliyor doğrulayın.
