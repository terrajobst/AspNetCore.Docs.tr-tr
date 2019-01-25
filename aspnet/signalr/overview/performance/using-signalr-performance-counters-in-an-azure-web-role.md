---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Bir Azure Web rolünde SignalR performans sayaçlarını kullanarak | Microsoft Docs
author: guardrex
description: Nasıl yükleyin ve bir Azure Web rolünde SignalR performans sayaçları kullanma.
keywords: ASP.NET,signalr,Performance sayacı, azure web rolü
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 8e17e945bc144731dd149bd7ddfc9e29160eaf0b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836495"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Bir Azure Web rolünde SignalR performans sayaçları kullanma

Tarafından [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

SignalR performans sayaçları, Azure Web rolünde uygulamanızın performansını izlemek için kullanılır. Sayaçlar, Microsoft Azure tanılama tarafından yakalanır. Azure ile SignalR performans sayaçları yüklediğiniz *signalr.exe*, tek başına veya şirket içi uygulamalar için kullanılan aynı aracı. Azure rolleri geçici olduğundan, bir uygulama yüklemek ve SignalR performans sayaçları başlatma kaydetmek için yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2015 veya [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Visual Studio için Microsoft Azure SDK'sı](https://azure.microsoft.com/downloads/) **Not: SDK'yı yükledikten sonra bilgisayarınızı yeniden başlatın.**
* Microsoft Azure aboneliği: Ücretsiz Azure deneme hesabı için kaydolmak için bkz: [Azure ücretsiz deneme](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>SignalR performans sayaçları sunan bir Azure Web rolü uygulama oluşturma

1. Visual Studio'yu açın.

2. Visual Studio'da **dosya** > **yeni** > **proje**.

3. İçinde **yeni proje** iletişim kutusunda **Visual C#** > **bulut** solda, kategori seçip **Azure bulut hizmeti** şablonu. Uygulamayı adlandırma **SignalRPerfCounters** seçip **Tamam**.

   ![Yeni bulut uygulaması](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Görmüyorsanız **bulut** şablon kategorisi veya **Azure bulut hizmeti** şablonu, yüklemeniz gereken **Azure geliştirme** Visual Studio 2017 için iş yükü. Seçin **açın, Visual Studio yükleyicisi** bağlantı sol alt tarafında **yeni proje** Visual Studio Yükleyicisi'ni açmak için iletişim. Seçin **Azure geliştirme** iş yükü ve ardından **Değiştir** iş yükü yüklemeye başlamak için.
   >
   > ![Visual Studio Yükleyicisi'deki Azure geliştirme iş yükü](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. İçinde **yeni Microsoft Azure bulut hizmeti** iletişim kutusunda **ASP.NET Web rolü** seçin > rolü projeye eklemek için düğme. **Tamam**’ı seçin.

   ![ASP.NET Web rolü ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. İçinde **yeni ASP.NET Web uygulaması - WebRole1** iletişim kutusunda **MVC** şablonu ve ardından **Tamam**.

   ![MVC ve Web API ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. İçinde **Çözüm Gezgini**açın *diagnostics.wadcfgx* altında dosya **WebRole1**.

   ![Solution Explorer diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Dosyanın içeriğini aşağıdaki yapılandırma ile değiştirin ve dosyayı kaydedin:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Açık **Paket Yöneticisi Konsolu** gelen **Araçları** > **NuGet Paket Yöneticisi**. SignalR ve SignalR yardımcı programları paketin en son sürümünü yüklemek için aşağıdaki komutları girin:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Başlatıldığında veya geri dönüştüren, rol örneğine SignalR performans sayaçlarını yüklemek üzere uygulamayı yapılandırır. İçinde **Çözüm Gezgini**, sağ **WebRole1** seçin ve proje **Ekle** > **yeni klasör**. Yeni klasör adı *başlangıç*.

   ![Başlangıç klasörü Ekle](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Kopyalama *signalr.exe* dosyası (eklenen **Microsoft.AspNet.SignalR.Utils** paket) öğesinden \<proje klasörü > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< Sürüm > / Araçları *başlangıç* önceki adımda oluşturduğunuz klasör.

11. İçinde **Çözüm Gezgini**, sağ *başlangıç* klasörü ve select **Ekle** > **varolan öğe**. Görüntülenen iletişim kutusunda, seçmek *signalr.exe* seçip **Ekle**.

    ![Signalr.exe projeye Ekle](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Sağ *başlangıç* oluşturduğunuz klasör. Seçin **ekleme** > **yeni öğe**. Seçin **genel** düğümünü **metin dosyası**ve yeni öğe adı *SignalRPerfCounterInstall.cmd*. Bu komut dosyasını web rolünde SignalR performans sayaçları yükler.

    ![SignalR performans sayacı yükleme toplu iş dosyasını oluşturun](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Visual Studio oluşturduğunda *SignalRPerfCounterInstall.cmd* dosyası ana penceresinde otomatik olarak açmak. Dosyanın içeriğini aşağıdaki betikle değiştirin, sonra kaydedin ve dosyayı kapatın. Bu betiği yürüten *signalr.exe*, rol örneği için SignalR performans sayaçları ekler.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Seçin *signalr.exe* dosyası **Çözüm Gezgini**. Dosyanın içinde **özellikleri**ayarlayın **çıkış dizinine Kopyala** için **her zaman Kopyala**.

    ![Her zaman Kopyala için çıktı dizinine Kopyala'ya ayarlayın](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. İçin önceki adımı yineleyin *SignalRPerfCounterInstall.cmd* dosya.


16. Sağ *SignalRPerfCounterInstall.cmd* seçin ve dosya **birlikte Aç**. Görüntülenen iletişim kutusunda, seçmek **ikili düzenleyici** seçip **Tamam**.

    ![İkili Düzenleyicisi'ni Aç](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. İkili Düzenleyici, tüm önde gelen bayt dosyasını seçin ve silebilirsiniz. Dosyayı kaydedin ve kapatın.

    ![Önde gelen bayt Sil](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Açık *ServiceDefinition.csdef* ve yürüten bir başlangıç görevi *SignalrPerfCounterInstall.cmd* dosya hizmeti başladığında:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Açık `Views/Shared/_Layout.cshtml` ve jQuery paket betik dosyanın sonundan kaldırın.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Sürekli olarak çağıran bir JavaScript istemcisi ekleme `increment` sunucuda yöntemi. Açık `Views/Home/Index.cshtml` ve içeriğini aşağıdaki kodla değiştirin:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Yeni bir klasör oluşturun **WebRole1** adlı proje *Hubs*. Sağ *Hubs* klasöründe **Çözüm Gezgini** seçip **Ekle** > **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web** > **SignalR** kategori tıklayın ve ardından **SignalR Hub sınıfı (v2)** öğe şablonu. Yeni hub'ı adı *MyHub.cs* seçip **Ekle**.

    ![Yeni Öğe Ekle iletişim kutusu Hubs klasörüne SignalR Hub sınıfı ekleme](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* ana penceresinde otomatik olarak açılır. İçeriğini aşağıdaki kodla değiştirin. daha sonra dosyayı kaydedip kapatın:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  olan bir bağlantı yoğunluğu testi aracı SignalR kod temeli ile sağlanan test. Mili kalıcı bir bağlantı gerektirdiğinden, sitenizde kullanmak için bir test ederken ekleyin. Yeni bir klasör eklemek **WebRole1** adlı proje *PersistentConnections*. Bu klasörü sağ tıklayıp **Ekle** > **sınıfı**. Yeni bir sınıf dosyası adı *MyPersistentConnections.cs* seçip **Ekle**.

24. Visual Studio açılır *MyPersistentConnections.cs* ana penceresinde dosya. İçeriğini aşağıdaki kodla değiştirin. daha sonra dosyayı kaydedip kapatın:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. Kullanarak `Startup` sınıfı, SignalR nesneleri Başlat OWIN başlatıldığında. Oluşturun veya açın *Startup.cs* ve içeriğini aşağıdaki kodla değiştirin:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    Yukarıdaki kodda `OwinStartup` özniteliği OWIN başlatmak için bu sınıfın işaretler. `Configuration` Yöntemi SignalR başlatır.

26. Tuşuna basarak uygulamanızı Microsoft Azure öykünücüsü'nde test **F5**.

    > [!NOTE]
    > Karşılaşırsanız bir **FileLoadException** adresindeki **MapSignalR**, bağlama yeniden yönlendirmelerini değiştirmek *web.config* aşağıdaki:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Yaklaşık bir dakika bekleyin. Visual Studio'da bulut Gezgini araç penceresi açın (**görünümü** > **Cloud Explorer**) yolunu genişletin `(Local)/Storage Accounts/(Development)/Tables`. Çift **WADPerformanceCountersTable**. Tablo verilerini SignalR sayaçları görmeniz gerekir. Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir. Seçmeniz gerekebilir **Yenile** tablosunda görmek için düğmeyi **Cloud Explorer** veya **Yenile** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesine.

    ![Visual Studio Cloud Explorer'da WAD performans sayaçları tabloyu seçme](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD performans sayaçları tablosunda toplanan gösteren sayaçlar](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Uygulamanızı bulutta test etmek için güncelleştirme **ServiceConfiguration.Cloud.cscfg** ayarlayın ve dosya `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` için geçerli bir Azure depolama hesabı bağlantı dizesi.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Azure aboneliğinizde uygulamayı dağıtın. Bir uygulamayı azure'a dağıtma hakkında daha fazla ayrıntı için bkz. [bir bulut hizmeti oluşturma ve dağıtma konusunda](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Birkaç dakika bekleyin. İçinde **Cloud Explorer**, yukarıda yapılandırdığınız depolama hesabını bulun ve bulma `WADPerformanceCountersTable` da tablo. Tablo verilerini SignalR sayaçları görmeniz gerekir. Tablo görmüyorsanız, Azure depolama kimlik bilgilerinizi yeniden girmeniz gerekebilir. Seçmeniz gerekebilir **Yenile** tablosunda görmek için düğmeyi **Cloud Explorer** veya **Yenile** tablosundaki verileri görmek için tabloyu Aç penceresinde düğmesine.

Özel performanstan [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) Bu öğreticide kullanılan özgün içerik için.
