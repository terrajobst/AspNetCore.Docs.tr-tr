---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: SignalR performans sayaçlarını bir Azure Web rolü kullanılarak | Microsoft Docs
author: guardrex
description: Nasıl yüklemek ve bir Azure Web rolünde SignalR performans sayaçlarını kullanın.
keywords: ASP.NET,signalr,Performance sayacı, azure web rolü
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2018
ms.locfileid: "28988022"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Bir Azure Web rolünde SignalR performans sayaçlarını kullanma

Tarafından [Luke Latham](https://github.com/guardrex)

SignalR performans sayaçlarını, bir Azure Web rolünde, uygulamanızın performansını izlemek için kullanılır. Sayaçlar, Microsoft Azure tanılama tarafından yakalanır. SignalR performans sayaçlarını ile azure'da yüklediğiniz *signalr.exe*, tek başına veya şirket içi uygulamalar için kullanılan aynı aracı. Azure rolleri geçici olduğundan, bir uygulama yüklemek ve başlangıç sırasında SignalR performans sayaçlarını kaydetmek için yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Visual Studio 2015 (VS2015) için Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/) **Not: SDK'yı yükledikten sonra bilgisayarınızı yeniden başlatın.**
* Microsoft Azure aboneliği: ücretsiz Azure deneme hesabı için kaydolmak için bkz: [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>SignalR performans sayaçlarını kullanıma sunan bir Azure Web rolü uygulama oluşturma

1. Visual Studio 2015'i açın.

2. Visual Studio 2015'te seçin **dosya** > **yeni** > **proje**.

3. İçinde **şablonları** bölmesinde **yeni proje** penceresinde altında **Visual C#** düğümü, select **bulut** düğümü ve select **Azure bulut hizmeti** şablonu. Uygulama adı **SignalRPerfCounters** seçip **Tamam**.

   ![Yeni bulut uygulaması](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. İçinde **yeni Microsoft Azure bulut hizmeti** iletişim kutusunda **ASP.NET Web rolü** ve seçin > projeye rolü eklemek için düğmesi. Seçin **Tamam**.

   ![ASP.NET Web rolü ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. İçinde **yeni ASP.NET Web uygulaması - WebRole1** iletişim kutusunda **MVC** şablonu ve ardından **Tamam**.

   ![MVC ve Web API ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. İçinde **Çözüm Gezgini**, açık *diagnostics.wadcfgx* altında dosya **WebRole1**.

   ![Solution Explorer diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Aşağıdaki yapılandırmaya sahip dosyasının içeriğini değiştirin ve dosyayı kaydedin:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Açık **Paket Yöneticisi Konsolu** gelen **Araçları** > **NuGet Paket Yöneticisi**. SignalR ve SignalR yardımcı programları paketi en son sürümünü yüklemek için aşağıdaki komutları girin:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Uygulama başlatıldığında veya geri dönüştürüldüğünde SignalR performans sayaçlarını rolü örneği yüklemek için yapılandırın. İçinde **Çözüm Gezgini**, sağ tıklayın **WebRole1** proje ve seçin **Ekle** > **yeni klasör**. Yeni bir klasör adı *başlangıç*.

   ![Başlangıç klasörü Ekle](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Kopya *signalr.exe* dosyası (eklendi **Microsoft.AspNet.SignalR.Utils** paket) gelen \<proje klasörü > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< Sürüm > / araçların *başlangıç* önceki adımda oluşturduğunuz klasör.

11. İçinde **Çözüm Gezgini**, sağ tıklatın *başlangıç* klasörü ve select **Ekle** > **varolan öğeyi**. Görüntülenen iletişim kutusunda, seçin *signalr.exe* seçip **Ekle**.

    ![Signalr.exe projeye ekleyin](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Sağ *başlangıç* oluşturduğunuz klasör. Seçin **ekleme** > **yeni öğe**. Seçin **genel** düğümü, select **metin dosyası**ve yeni öğe adını *SignalRPerfCounterInstall.cmd*. Bu komut dosyasını web rolünde SignalR performans sayaçlarını yükler.

    ![SignalR performans sayacı yükleme toplu iş dosyası oluşturma](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Visual Studio oluşturduğunda *SignalRPerfCounterInstall.cmd* dosyası, ana penceresinde otomatik olarak açmak. Aşağıdaki komut dosyası ile dosyasının içeriğini değiştirin sonra dosyasını kaydedin ve kapatın. Bu betiği yürüten *signalr.exe*, rol örneği için SignalR performans sayaçlarını ekler.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Seçin *signalr.exe* dosyasını **Çözüm Gezgini**. Dosyanın içinde **özellikleri**ayarlayın **çıktı dizinine Kopyala** için **her zaman Kopyala**.

    ![Her zaman kopyalamak için çıktı dizinine Kopyala ayarlayın](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. İçin önceki adımı yineleyin *SignalRPerfCounterInstall.cmd* dosya.

    
16. Sağ *SignalRPerfCounterInstall.cmd* dosya ve seçin **birlikte Aç**. Görüntülenen iletişim kutusunda, seçin **İkili Düzenleyicisi** seçip **Tamam**.

    ![İkili Düzenleyicisi ile Aç](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. İkili Düzenleyicisi'nde dosyasındaki tüm önde gelen bayt seçmek ve silebilirsiniz. Dosyayı kaydedin ve kapatın.

    ![Önde gelen bayt Sil](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Açık *ServiceDefinition.csdef* ve yürüten bir başlangıç görevi *SignalrPerfCounterInstall.cmd* dosya hizmeti başladığında:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Açık `Views/Shared/_Layout.cshtml` ve jQuery paket komut dosyası sonundan kaldırın.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Sürekli olarak çağıran bir JavaScript istemcisi ekleme `increment` sunucuda yöntemi. Açık `Views/Home/Index.cshtml` ve içeriğini aşağıdaki kodla değiştirin:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Yeni bir klasör oluşturun **WebRole1** adlı projesi *hub*. Sağ *hub* klasöründe **Çözüm Gezgini**seçin **Web** > **SignalR**seçip  **SignalR hub'ı sınıfı (v2)**. Yeni hub'ı adı *MyHub.cs* seçip **Ekle**.

    ![Yeni Öğe Ekle iletişim kutusu hub klasöründe SignalR hub'ı sınıfı ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* ana penceresinde otomatik olarak açılır. İçeriğini aşağıdaki kodla değiştirin sonra dosyayı kaydedip kapatın:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  olan SignalR codebase ile sağlanan aracını test bağlantısı yoğunluğu. Mili kalıcı bir bağlantı gerektirdiğinden, sitenize kullanmak için bir tane sınarken ekleyin. Yeni bir klasör ekleyin **WebRole1** adlı proje *PersistentConnections*. Bu klasörü sağ tıklatın ve seçin **Ekle** > **sınıfı**. Yeni sınıf dosya adı *MyPersistentConnections.cs* seçip **Ekle**.

24. Visual Studio açılır *MyPersistentConnections.cs* ana penceresinde dosya. İçeriğini aşağıdaki kodla değiştirin sonra dosyayı kaydedip kapatın:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Kullanarak `Startup` sınıfı, SignalR nesneleri Başlat OWIN başladığında. Açmaya veya oluşturmaya *haline* ve içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    Yukarıdaki kod `OwinStartup` özniteliği OWIN başlatmak için bu sınıf işaretler. `Configuration` Yöntemi SignalR başlatır.
    
26. Tuşlarına basarak uygulamanızı Microsoft Azure öykünücüsünde test **F5**.

    > [!NOTE]
    > Karşılaşırsanız bir **FileLoadException** adresindeki **MapSignalR**, bağlama yeniden yönlendirmeleri içinde değiştirmek *web.config* şu:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Yaklaşık bir dakika bekleyin. Visual Studio'da Cloud Explorer araç penceresi açın (**Görünüm** > **Cloud Explorer**) ve yolun genişletin `(Local)/Storage Accounts/(Development)/Tables`. Çift **WADPerformanceCountersTable**. Tablo verisi SignalR sayaçları görmeniz gerekir. Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir. Seçmeniz gerekebilir **yenileme** tablosunda görmek için düğmesi **Cloud Explorer** veya seçin **yenileme** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesini.

    ![Visual Studio Cloud Explorer'da WAD performans sayaçları tablo seçme](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD performans sayaçları tabloda toplanan sayaçları gösterme](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Bulutta uygulamanızı test etmek için Güncelleştir **ServiceConfiguration.Cloud.cscfg** dosya ve ayarlayın `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` geçerli bir Azure depolama hesabı bağlantı dizesi.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Azure aboneliğiniz uygulamayı dağıtın. Azure için bir uygulamanın nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [nasıl oluşturulacağı ve bir bulut hizmetinin dağıtılacağı](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Birkaç dakika bekleyin. İçinde **Cloud Explorer**, yukarıda yapılandırılan depolama hesabını bulun ve bulmak `WADPerformanceCountersTable` bu tabloda. Tablo verisi SignalR sayaçları görmeniz gerekir. Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir. Seçmeniz gerekebilir **yenileme** tablosunda görmek için düğmesi **Cloud Explorer** veya seçin **yenileme** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesini.

Özel teşekkürler [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) Bu öğreticide kullanılan özgün içerik için.
