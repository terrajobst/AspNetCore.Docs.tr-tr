---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Azure Service Bus ile SignalR ölçeğini genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 751de591ec62942c2c9d8b26af40101eb2e922d5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820946"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a>Azure Service Bus ile SignalR ölçeğini genişletme (SignalR 1.x)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Bu öğreticide, bir Windows Azure Web her rol örneği iletilerini dağıtmak için Service Bus devre kartına kullanarak rol bir SignalR uygulamayı dağıtacaksınız.

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Önkoşullar:

- Bir Windows Azure hesabı.
- [Windows Azure SDK'sı](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012.

Service bus devre kartı da uyumlu [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), sürüm 1.1. Ancak, Windows Server için hizmet veri yolu 1.0 sürümü ile uyumlu değil.

## <a name="pricing"></a>Fiyatlandırma

Service Bus devre kartına ileti göndermek için konulara kullanır. En son fiyatlandırma bilgileri için bkz. [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). Bu makalenin yazıldığı sırada, kısa $1 için ayda 1.000.000 iletileri gönderebilir. Devre kartına bir SignalR hub'yöntemini her çalıştırılışı için hizmet veri yolu ileti gönderir. Bağlantı, bağlantı kesilmesi, katılma veya bırakma grupları ve benzeri için bazı denetim iletileri de vardır. Çoğu uygulamada ileti trafiği çoğunu hub yöntemi çağrılarına olacaktır.

## <a name="overview"></a>Genel Bakış

Ayrıntılı Öğreticisine aldığımız önce ne yapacağını, hızlı bir genel bakış aşağıda verilmiştir.

1. Yeni bir Service Bus ad alanı oluşturmak için Windows Azure portal'ı kullanın.
2. Bu NuGet paketlerini uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.ServiceBus](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Her uygulama için "Uygulamanızınadı" için farklı bir değer seçin. Aynı değeri, birden çok uygulamada kullanmayın.

## <a name="create-the-azure-services"></a>Azure hizmetleri oluşturma

Bölümünde anlatıldığı gibi bir bulut hizmeti oluşturma [bir bulut hizmeti oluşturma ve dağıtma konusunda](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Bölümündeki adımları "nasıl yapılır: hızlı Oluştur kullanarak bir bulut hizmeti oluşturma". Bu öğreticide, bir sertifikayı karşıya yüklemek gerekmez.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Açıklanan şekilde yeni bir Service Bus ad alanı oluşturma [nasıl kullanım hizmet veri yolu konuları/abonelikleri için](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). "Hizmet Namespace oluşturma" bölümündeki adımları izleyin.

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Bulut hizmeti ve Service Bus ad alanı için aynı bölge seçtiğinizden emin olun.


## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio'yu başlatın. Gelen **dosya** menüsünü tıklatın **yeni proje**.

İçinde **yeni proje** iletişim kutusunda **Visual C#**. Altında **yüklü şablonlar**seçin **bulut** seçip **Windows Azure bulut hizmeti**. Varsayılan .NET Framework 4.5 tutun. ChatService uygulamaya bir ad ve tıklayın **Tamam**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

İçinde **yeni Windows Azure bulut hizmeti** iletişim kutusunda seçim ASP.NET MVC 4 Web rolü. Sağ ok düğmesine tıklayın (**&gt;**) rolünü çözümünüze ekleyin.

Fareyi yeni rol, bu nedenle gelin Kurşun Kalem simgesi görünür. Rolü yeniden adlandırmak için bu simgeye tıklayın. Rol "SignalRChat" adını verin ve tıklayın **Tamam**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

İçinde **yeni ASP.NET MVC 4 proje** seçin **Internet uygulaması**. **Tamam**'ı tıklatın. Proje Sihirbazı, iki proje oluşturur:

- ChatService: Bu proje, Windows Azure uygulamasıdır. Bu, Azure rolleri ve diğer yapılandırma seçenekleri tanımlar.
- SignalRChat: Bu, ASP.NET MVC 4 Proje projesidir.

## <a name="create-the-signalr-chat-application"></a>SignalR sohbet uygulaması oluşturma

Sohbet uygulaması oluşturmak için öğreticinin adımları [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).

Gerekli kitaplıkları yükleme için NuGet kullanın. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutları girin:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Kullanım `-ProjectName` Windows Azure projesi yerine ASP.NET MVC projesi için paketleri yüklemek için seçeneği.

## <a name="configure-the-backplane"></a>Devre kartına yapılandırın

Uygulamanızın Global.asax dosyasında aşağıdaki kodu ekleyin:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Artık service bus bağlantı dizenizi almanız gerekir. Azure portalında, oluşturduğunuz hizmet veri yolu ad alanını seçin ve erişim anahtarı simgesine tıklayın.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

Bağlantı dizesini panoya kopyalayın ve yapıştırın *connectionString* değişkeni.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Çözüm Gezgini'nde **rolleri** ChatService proje klasöründen.

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

SignalRChat role sağ tıklayıp **özellikleri**. Seçin **yapılandırma** sekmesi. Altında **örnekleri** 2'yi seçin. Ayrıca VM boyutu ayarlayabileceğiniz **çok küçük**.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Değişiklikleri kaydedin.

Çözüm Gezgini'nde ChatService projeye sağ tıklayın. Seçin **yayımlama**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Bu, ilk saat yayımlama Windows azure'a ise, kimlik bilgilerinizi indirmeniz gerekir. İçinde **Yayımla** Sihirbazı, "kimlik bilgilerini indirmek oturum"'a tıklayın. Bu, Windows Azure portalında oturum açıp bir yayımlama ayarları dosyasını indirin ister.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Tıklayın **alma** ve indirdiğiniz yayımlama ayarları dosyası seçin.

**İleri**'ye tıklayın. İçinde **yayımlama ayarları** iletişim altında **bulut hizmeti**, daha önce oluşturduğunuz bir bulut hizmeti seçin.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Tıklayın **yayımlama**. Uygulamanın, uygulamayı dağıtmak ve sanal makinelerin başlamak için birkaç dakika sürebilir.

Şimdi Sohbet uygulamayı çalıştırdığınızda, rol örneklerini bir Service Bus konusu kullanarak Azure Service Bus ile iletişim kurar. Birden çok aboneyi izin veren bir ileti kuyruğu bir konudur.

Devre kartına, konu ve abonelik otomatik olarak oluşturur. Abonelikler ve ileti etkinliği görmek için Azure portalını açın, Service Bus ad alanını seçin ve "Konularda"'a tıklayın.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Bunu yapmak Panoda gösterilecek ileti etkinliği için birkaç dakika sürer.

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

SignalR konu yaşam süresini yönetir. Uygulamanız dağıtıldığında olduğu sürece, konu ayarlarını el ile konuları silin veya çalışmayın.
