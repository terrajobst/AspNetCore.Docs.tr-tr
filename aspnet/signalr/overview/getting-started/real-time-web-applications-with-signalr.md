---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Uygulamalı Laboratuvar: SignalR ile gerçek zamanlı Web uygulamaları | Microsoft Docs'
author: rick-anderson
description: Gerçek zamanlı Web uygulamaları, sunucu tarafı, gerçek zamanlı olarak ortaya çıktığı gibi bağlı istemcilere içerik gönderme olanağı özellik. ASP, ASP.NET geliştiricileri için...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 50ed2bed6b5b20684d00d7887494ee41346b5c3f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828915"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Uygulamalı Laboratuvar: SignalR ile gerçek zamanlı Web uygulamaları
====================
Tarafından [Team Web Kampları](https://twitter.com/webcamps)

[Eğitim Seti Web Kampları indirin](http://aka.ms/webcamps-training-kit)

> Gerçek zamanlı Web uygulamaları, sunucu tarafı, gerçek zamanlı olarak ortaya çıktığı gibi bağlı istemcilere içerik gönderme olanağı özellik. ASP.NET geliştiricilerine yönelik **ASP.NET SignalR** uygulamalarına gerçek zamanlı web işlevselliği eklemek için bir kitaplıktır. Otomatik olarak verilen istemci ve sunucunun en iyi kullanılabilir aktarım en iyi kullanılabilir taşıma seçme birkaç aktarımı avantajlarından yararlanır. Avantajlarından yararlanır **WebSocket**, tarayıcı ve sunucu arasında çift yönlü iletişimi sağlayan HTML5 API.
> 
> **SignalR** ayrıca istemciye RPC sunucusu yapmak için basit, üst düzey bir API sağlar. (sunucu tarafı .NET kodundan müşterilerinizin tarayıcılarında JavaScript işlevleri çağırmak) ASP.NET uygulamanızı, aynı zamanda bağlantı yönetimi, kullanışlı kancaları ekleme olayları bağlama/bağlantısını kes, bağlantıları gruplandırma ve yetkilendirme gibi.
> 
> **SignalR** bazı istemci ve sunucu arasında gerçek zamanlı iş yapmak için gerekli olan taşımalar üzerinde bir soyutlamadır. A **SignalR** bağlantı HTTP başlar ve sonra yükseltilen bir **WebSocket** bağlantı varsa. **WebSocket** için ideal aktarım **SignalR**, sunucu bellek en verimli bir şekilde kullanılmasını kolaylaştırır. bu yana varsa gecikme süresi en düşük ve en alttaki özellikler (gibi istemci arasındaki tam çift yönlü iletişim ve sunucu için), ancak ayrıca en katı gereksinimleri vardır: **WebSocket** sunucusu kullanılmasını gerektirir **Windows Server 2012** veya **Windows 8**, yanısıra **.NET framework 4.5**. Bu gereksinimler karşılanmazsa **SignalR** bağlantılarından olmak için diğer taşımalar kullanmayı dener (gibi *Ajax uzun yoklama*).
> 
> **SignalR** API içeren istemciler ve sunucular arasında iletişim kurmak için iki modeli: **kalıcı bağlantılar** ve **Hubs**. A **bağlantı** tek alıcısı, gönderme gruplandırılmış veya yayın iletileri için basit bir uç noktasını temsil eder. A **Hub** olduğundan, istemci ve sunucunun doğrudan birbirleri üzerinde yöntemleri çağırmak verir bağlantı API üzerinde derlenmiş daha üst düzey bir işlem hattı.
> 
> ![SignalR mimarisi](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:

- Bildirimleri, sunucudan istemciye SignalR kullanarak gönderin.
- SignalR kullanarak uygulama ölçek **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya üzeri

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı laboratuvarda alıştırmalar çalıştırmak için önce ortamı oluşturmanız gerekir.

1. Bir Windows Explorer penceresi açın ve Laboratuvar için Gözat **kaynak** klasör.
2. Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırın ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemini başlatmak için.
3. Kullanıcı Hesabı Denetimi iletişim kutusunu gösteriliyorsa, devam etmek için eylemi onaylayın.

> [!NOTE]
> Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların etkinleştirdiğinizden emin olun.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Kod parçacıklarını kullanma

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.

> [!NOTE]
> Her alıştırma bulunan bir başlangıç çözüm eşlik **başlamak** her alıştırma diğerlerinden takip etmenize olanak tanıyan çalışma klasörü. Lütfen bir alıştırma sırasında eklenen kod parçacıkları bu çözümleri başlangıç eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın. Ayrıca bulabilirsiniz bir alıştırma için kaynak kod içinde bir **son** karşılık gelen bir alıştırma olarak adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözüm içeren klasör. Bu uygulamalı laboratuvarı çalışırken ek yardıma ihtiyacınız varsa, bu çözümleri kılavuz kullanabilirsiniz.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:

1. [SignalR kullanarak gerçek zamanlı verilerle çalışma](#Exercise1)
2. [SQL Server kullanarak ölçeklendirme](#Exercise2)

Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**

> [!NOTE]
> Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir. Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Alıştırma 1: SignalR kullanarak gerçek zamanlı verilerle çalışma

Sohbet genellikle bir örnek olarak kullanılırken, bütün yapabileceğiniz çok daha ile gerçek zamanlı Web işlevselliği. Bir kullanıcı yeni veriler veya uzun yoklama yeni verileri almak için bir Ajax sayfasına uygulayan bir web sayfası her yenilendiğinde SignalR kullanabilirsiniz.

SignalR destekler **sunucu gönderimi** veya **yayın** işlevselliği; bağlantı yönetimi otomatik olarak işler. Klasik HTTP bağlantılarında istemci-sunucu iletişimi için bağlantı her istek için yeniden kurulur, ancak SignalR istemci ve sunucu arasında kalıcı bir bağlantı sağlar. Sunucu kodu tarayıcıda uzak yordam çağrılarını (RPC) kullanarak bir istemci kodu çağıran SignalR öğesinde istek-yanıt modeli yerine bugün biliyoruz.

Bu alıştırmada yapılandıracağınız **Geek test** güncelleştirilmiş Ölçümler ve tüm sayfanın yenilenmesi gerekmeden istatistikleri panoyu görüntülemek için SignalR kullanılacak uygulama.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Görev 1 – Geek test istatistikleri sayfası keşfetme

Bu görevde, uygulamasına gidin ve istatistikleri sayfanın nasıl gösterildiğini doğrulayın ve bilgileri şekilde nasıl geliştireceğiniz güncelleştirilir.

1. Açık **Visual Studio Express 2013 Web** açın **GeekQuiz.sln** çözüm bulunan **Source\Ex1 WorkingWithRealTimeData\Begin** klasör.
2. Tuşuna **F5** çözümü çalıştırın. **Oturum** sayfası, tarayıcıda görüntülenmelidir.

    ![Çözüm çalıştırılmadan](real-time-web-applications-with-signalr/_static/image2.png "çözümü çalıştırma")

    *Çözüm çalıştırma*
3. Tıklayın **kaydetme** uygulamada yeni bir kullanıcı oluşturmak için sayfanın üst sağ üst köşedeki.

    ![Bağlantı kaydetme](real-time-web-applications-with-signalr/_static/image3.png "Kaydet bağlantısına")

    *Bağlantı kaydetme*
4. İçinde **kaydetme** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetme**.

    ![Bir kullanıcıyı kaydediyor](real-time-web-applications-with-signalr/_static/image4.png "kullanıcı kaydetme")

    *Bir kullanıcı kaydetme*
5. Uygulama, yeni hesabı kaydeder ve kullanıcı kimlik doğrulaması ve geri ilk test soru gösteren giriş sayfasına yeniden yönlendirildi.
6. Açık **istatistikleri** sayfasında yeni bir pencerede ve put **giriş** sayfası ve **istatistikleri** yan yana sayfasında.

    ![Yan yana windows](real-time-web-applications-with-signalr/_static/image5.png "windows yan yana")

    *Yan yana windows*
7. İçinde **giriş** sayfasında, seçeneklerden birine tıklayarak soruyu yanıtlayın.

    ![Bir soruyu yanıtlarken](real-time-web-applications-with-signalr/_static/image6.png "soru yanıtlama")

    *Bir soru yanıtlama*
8. Düğmelerden birine tıklandıktan sonra yanıt görüntülenmesi gerekir.

    ![Sorunun yanıtlandığını doğru](real-time-web-applications-with-signalr/_static/image7.png "sorusuna cevap doğru")

    *Sorunuzun doğru*
9. İstatistikleri sayfasında sağlanan bilgileri güncel olduğuna dikkat edin. Güncelleştirilmiş sonuçları görmek için sayfayı yenileyin.

    ![İstatistikleri sayfası](real-time-web-applications-with-signalr/_static/image8.png "istatistikleri sayfası")

    *İstatistikleri sayfası*
10. Visual Studio'ya geri dönün ve hata ayıklamayı durdurun.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Görev 2 – çevrimiçi grafikleri gösterecek şekilde Geek sınavı için ekleme SignalR

Bu görevde, SignalR çözüme eklemek ve sunucuda yeni bir yanıt gönderildiğinde güncelleştirmelerini istemcilere otomatik olarak gönder.

1. Gelen **Araçları** Visual Studio'da seçim menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
2. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu yürütün:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR paket yüklemesi](real-time-web-applications-with-signalr/_static/image9.png "SignalR paket yükleme")

    *SignalR paket yükleme*

   > [!NOTE]
   > Yüklerken **SignalR** NuGet paketleri sürüm 2.0.2 yepyeni bir MVC 5 uygulamasında el ile güncelleştirmeniz gerekecek **OWIN** paketleri 2.0.1 sürümüne (veya üzeri) SignalR yüklemeden önce. Bunu yapmak için aşağıdaki betiği yürütebilirsiniz **Paket Yöneticisi Konsolu**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > Gelecekteki bir SignalR sürümünde, OWIN bağımlılıkları otomatik olarak güncelleştirilir.
3. İçinde **Çözüm Gezgini**, genişletin **betikleri** klasörü ve bildirimi, SignalR *js* dosyaları çözüme eklendi.

    ![SignalR JavaScript başvuran](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript başvuruyor")

    *SignalR JavaScript başvuruları*
4. İçinde **Çözüm Gezgini**, sağ **GeekQuiz** proje, select **Ekle** | **yeni klasör**, adlandırın **Hub'ları**.
5. Sağ **Hubs** klasörü ve select **Ekle | Yeni öğe**.

    ![Yeni Öğe Ekle](real-time-web-applications-with-signalr/_static/image11.png "Yeni Öğe Ekle")

    *Yeni Öğe Ekle*
6. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# | Web | SignalR** select sol bölmedeki düğüm **SignalR Hub sınıfı (v2)** Orta bölmede dosya adı **StatisticsHub.cs** tıklatıp **Ekle**.

    ![Yeni öğe iletişim kutusu Ekle](real-time-web-applications-with-signalr/_static/image12.png "yeni öğe iletişim kutusu Ekle")

    *Yeni Öğe Ekle iletişim kutusu*
7. Değiştirin **StatisticsHub** aşağıdaki kodla sınıfı.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Açık **Startup.cs** ve en sonunda şu satırı ekleyin **yapılandırma** yöntemi.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Açık **StatisticsService.cs** içinde sayfa **Hizmetleri** klasörü ve aşağıdaki yönergeleri kullanarak.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Güncelleştirmelerin bağlı istemciler bildirmek için önce aldığınız bir **bağlam** geçerli bağlantı için nesne. **Hub** nesne tek bir istemci veya yayın için tüm bağlı istemcileri iletileri göndermek için yöntemleri içerir. Aşağıdaki yöntemi ekleyin **StatisticsService** istatistik verilerini yayınlamak için sınıf.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Yukarıdaki kodda, istemcide bir işlevi çağırmak için bir rastgele yöntem adı kullanıyorsanız (örn: *updateStatistics*). Belirttiğiniz yöntem adı, IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelen dinamik bir nesne olarak yorumlanır. İfade, çalışma zamanında değerlendirilir. Yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değerlerini istemciye gönderir. İstemci adıyla eşleşen bir yöntemi varsa, bu yöntem çağrılır ve parametre değerlerini kendisine geçirilir. Eşleşen hiçbir yöntemi istemcide bulunursa, herhangi bir hata ortaya çıkar. Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu](../guide-to-the-api/hubs-api-guide-server.md).
11. Açık **TriviaController.cs** içinde sayfa **denetleyicileri** klasörü ve aşağıdaki yönergeleri kullanarak.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Aşağıdaki vurgulanmış kodu ekleyin **Post** eylem yöntemi.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Açık **Statistics.cshtml** içinde sayfa **görünümleri | Giriş** klasör. Bulun **betikleri** bölümünde ve bölümün başında aşağıdaki betik başvurularını ekleyin.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Visual Studio projenize SignalR ve diğer betik kitaplıkları eklemek, Paket Yöneticisi SignalR betik dosyasının bu konuda gösterilen sürümden daha yeni bir sürümü yükleyebilirsiniz. Kodunuzda betik başvurusu betik kitaplığını projenize yüklü sürümü eşleştiğinden emin olun.
14. İstemci SignalR hub'ına bağlanma ve hub'ın yeni bir ileti alındığında, istatistik verileri güncelleştirmek için aşağıdaki vurgulanmış kodu ekleyin.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    Bu kodda bir Hub Proxy oluşturma ve sunucu tarafından gönderilen iletileri dinlemek için bir olay işleyicisi kaydediliyor. Bu durumda, aracılığıyla gönderilen iletiler için dinleme *updateStatistics* yöntemi.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Görev 3: çözümü çalıştırma

Bu görevde, otomatik olarak yeni bir soru yanıtlama sonra SignalR kullanarak, istatistikleri görünümü güncelleştirildiğini doğrulamak için çözüm çalışır.

1. Tuşuna **F5** çözümü çalıştırın.

    > [!NOTE]
    > Görev 1'de oluşturduğunuz kullanıcı uygulamaya zaten oturum oturum.
2. Açık **istatistikleri** sayfasında yeni bir pencerede ve put **giriş** sayfası ve **istatistikleri** görev 1'de yaptığınız gibi yan yana sayfasında.
3. İçinde **giriş** sayfasında, seçeneklerden birine tıklayarak soruyu yanıtlayın.

    ![Başka bir soruyu yanıtlarken](real-time-web-applications-with-signalr/_static/image13.png "başka bir soru yanıtlama")

    *Başka bir soru yanıtlama*
4. Düğmelerden birine tıklandıktan sonra yanıt görüntülenmesi gerekir. Güncelleştirilmiş bilgileri ve tüm sayfanın yenilenmesi gerekmeden sorusu yanıtlama sonra sayfasında istatistik bilgilerini otomatik olarak güncelleştirilir dikkat edin.

    ![İstatistikleri sayfası yenilenir sonra yanıt](real-time-web-applications-with-signalr/_static/image14.png "istatistikleri sayfası yenilenir sonra yanıt")

    *Yanıttan sonra yenilenmesi istatistikleri sayfası*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Alıştırma 2: SQL Server kullanarak ölçeklendirme

Bir web uygulaması ölçeklendirme, genellikle arasından seçim yapabilirsiniz *büyütme* ve *ölçeği genişletme* seçenekleri. *Ölçeği artırma* sırasında daha fazla kaynağa (CPU, RAM, vb.) daha büyük bir sunucu kullanmak anlamına gelir *ölçeğini* yükü işlemek için daha fazla sunucu eklemek anlamına gelir. İstemciler farklı sunuculara yönlendirilir ikinci bir sorun var. Bir sunucuya bağlı bir istemci, başka bir sunucudan gönderilen iletileri almazsınız.

Adlı bir bileşen kullanarak bu sorunları çözebilir *devre kartı*, sunucular arasında iletileri iletecek şekilde. Etkin bir devre kartı ile her uygulama örneği devre kartına ileti gönderir ve devre kartına bunları diğer uygulama örnekleri iletir.

Şu anda backplanes SignalR için üç tür vardır:

- **Windows Azure Service Bus**. Service Bus bileşenleri gevşek ileti göndermek izin veren bir Mesajlaşma altyapısıdır.
- **SQL Server**. SQL Server devre kartına ileti SQL tablolarının yazar. Devre kartına verimli Mesajlaşma için hizmet Aracısı kullanır. Hizmet Aracısı etkin değilse ancak, bu da çalışır.
- **Redis**. Redis bellek içi anahtar-değer deposudur. Redis ileti göndermek için bir yayımlama/abone olma ("pub/sub") deseni destekler.

Her ileti bir ileti veri yolu gönderilir. Bir ileti veri yoluna uygulayan [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) arabirimi, yayımlama/abone olma bir Özet sağlar. Varsayılan değiştirerek backplanes çalışma **IMessageBus** o devre kartı için tasarlanan veri.

Her sunucu örneği devre kartına Veriyolu aracılığıyla bağlanır. Bir ileti gönderildiğinde devre kartına gider ve isteğe bağlı olarak devre kartına her sunucuya gönderir. Bir sunucu devre kartından bir ileti aldığında, iletiyi kendi yerel önbellekte depolar. Sunucu, bunun yerel önbelleğinden istemciler için iletileri ardından sunar.

SignalR devre kartına, bunu okuyun işleyişi hakkında daha fazla bilgi için [makale](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Burada bir devre kartı bir performans sorunu haline gelebilir bazı senaryolar vardır. Bazı tipik SignalR senaryolar aşağıda verilmiştir:
> 
> - [Sunucu yayın](tutorial-server-broadcast-with-signalr.md) (örneğin, bandı): Backplanes sunucu iletileri gönderilir oranı denetlediğinden bu senaryo için iyi çalışır.
> - [İstemci istemci](tutorial-getting-started-with-signalr.md) (örneğin, sohbet edin): ileti sayısını ölçeklendirir istemci sayısı, bu senaryoda, bir performans sorunu devre kartına olabilir; diğer bir deyişle, orantılı olarak daha fazla istemciye iletileri oranı büyürse katılın.
> - [Yüksek sıklıkta gerçek zamanlı](tutorial-high-frequency-realtime-with-signalr.md) (örneğin, gerçek zamanlı oyun): Bu senaryo için bir devre kartı önerilmez.


Bu alıştırmada, kullanacağınız **SQL Server** iletilerini arasında dağıtmak için **Geek test** uygulama. Bu görevleri öğrenmenin yapılandırmayı ayarlamak için ancak tam etkiyi görmek için bir tek test makinesinde çalışır, iki veya daha fazla sunucu SignalR uygulamayı dağıtmak ihtiyacınız olacak. SQL Server sunuculardan biri üzerinde veya ayrılmış ayrı bir sunucuya yüklemeniz gerekir.

![SQL Server diyagramı kullanarak ölçek](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Görev 1 - senaryo anlama

Bu görevde, 2 örneklerini çalıştıracağınız **Geek test** örnekler yerel makinenizde birden fazla IIS benzetimi. Bu senaryoda, bir uygulama üzerinde Trivia sorularını yanıtlama, ikinci örnek istatistik sayfasında güncelleştirme bildirilmez. Uygulamanızı birden çok örnek üzerinde dağıtıldığı bir ortamda bu simülasyon benzer ve bunlarla iletişim kurmak için bir yük dengeleyici kullanma.

1. Açık **Begin.sln** çözüm bulunan **kaynak/Ex2-ScalingOutWithSQLServer/başlangıcı** klasör. Veriler yüklendikten sonra üzerinde fark edeceksiniz **Sunucu Gezgini** çözüm iki proje ile aynı olduğunu, ancak farklı adlar yapıları. Bu, iki örneği aynı uygulamayı çalıştıran yerel makinenizde benzetimini yapacaksınız.

    ![Çözüm Geek test 2 örneklerini benzetimi başlatmak](real-time-web-applications-with-signalr/_static/image16.png "başlamak çözüm Geek test 2 örneklerini benzetimi")

    *Geek test 2 örneklerini benzetimi çözüm başlayın*
2. Çözüm düğümüne sağ tıklatıp seçerek çözümün özellikler sayfasını açın **özellikleri**. Altında **başlangıç projesi**seçin **birden fazla başlangıç projesi** değiştirip **eylem** iki proje için değer *Başlat*.

    ![Birden çok proje başlangıç](real-time-web-applications-with-signalr/_static/image17.png "birden çok proje başlatılıyor")

    *Birden çok proje başlatılıyor*
3. Tuşuna **F5** çözümü çalıştırın. Uygulama iki örneği başlatacak **Geek test** aynı uygulamanın birden çok örneğini farklı bağlantı noktalarını simüle. Sol taraftaki ekranınızın sağ taraftaki diğer tarayıcılardan birini sabitleyin. Kimlik bilgilerinizle oturum açın veya yeni bir kullanıcı kaydı. Oturum açtıktan sonra Meraklısına Notlar sayfanın sol tarafta tutun ve Git **istatistikleri** sağdaki tarayıcıdaki sayfası.

    ![Geek test yan yana](real-time-web-applications-with-signalr/_static/image18.png)

    *Geek test yan yana*

    ![Geek test farklı bağlantı noktaları](real-time-web-applications-with-signalr/_static/image19.png)

    *Geek test farklı bağlantı noktaları*
4. Sol tarayıcıda soru yanıtlama başlatın ve göreceksiniz **istatistikleri** sağdaki tarayıcıdaki sayfa değil güncelleştiriliyor. Bunun nedeni, **SignalR** kullanan yerel bir önbellek arasında istemcileri iletilerini dağıtmak için ve bu senaryo, birden çok örneği benzetimi, bu nedenle bir önbellek bunlar arasında paylaşılmaz. Doğrulayabilirsiniz **SignalR** aynı adımları test ancak tek bir uygulama kullanarak çalışmaktadır. Aşağıdaki görevleri örneklerinde iletileri çoğaltmak için bir devre kartı yapılandıracaksınız.
5. Visual Studio'ya geri dönün ve hata ayıklamayı durdurun.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Görev 2: SQL Server devre kartı oluşturma

Bu görevde, için bir devre kartı olarak hizmet verecek bir veritabanı oluşturacaksınız **Geek test** uygulama. Kullanacağınız **SQL Server Nesne Gezgini** sunucunuza gidin ve veritabanını başlatmak için. Ayrıca, sağlayacak **hizmet Aracısı**.

1. İçinde **Visual Studio**açın menü **görünümü** seçip **SQL Server Nesne Gezgini**.
2. Sağ tıklayarak LocalDB Örneğinize bağlanmak **SQL Server** düğüm ve seçerek **SQL Server Ekle...**  seçeneği.

    ![Bir SQL Server örneği ekleme](real-time-web-applications-with-signalr/_static/image20.png "bir SQL Server örneği ekleme")

    *Bir SQL Server örneği için SQL Server Nesne Gezgini ekleme*
3. Ayarlama **sunucu adı** için *(localdb) \v11.0* bırakıp **Windows kimlik doğrulaması** , kimlik doğrulama modu. Tıklayın **Connect** devam etmek için.

    ![Bağlanmak için LocalDB](real-time-web-applications-with-signalr/_static/image21.png "yerel veritabanı'na bağlanma")

    *Yerel veritabanı'na bağlanma*
4. LocalDB Örneğinize bağlı, SignalR için SQL Server devre kartı temsil edecek bir veritabanı oluşturmanız gerekir. Bunu yapmak için sağ **veritabanları** düğümünü seçip alt **yeni veritabanı Ekle**.

    ![Yeni bir veritabanı ekleme](real-time-web-applications-with-signalr/_static/image22.png "yeni bir veritabanı ekleme")

    *Yeni bir veritabanı ekleme*
5. Veritabanı adı kümesine *SignalR* tıklatıp **Tamam** oluşturun.

    ![SignalR veritabanı oluşturma](real-time-web-applications-with-signalr/_static/image23.png "SignalR veritabanı oluşturma")

    *SignalR veritabanı oluşturma*

    > [!NOTE]
    > Veritabanı için herhangi bir ad seçebilirsiniz.
6. Güncelleştirmeleri kartından daha verimli bir şekilde almak için bu veritabanı için hizmet Aracısı etkinleştirme önerilir. Hizmet Aracısı ileti ve SQL Server'da queuing için yerel destek sağlar. Devre kartına de hizmet aracısı çalışır. Yeni bir sorgu seçin ve veritabanı sağ tıklayarak açın **yeni sorgu**.

    ![Yeni bir sorgu açarak](real-time-web-applications-with-signalr/_static/image24.png "yeni bir sorgu açma")

    *Yeni bir sorgu açma*
7. Hizmet Aracısı etkin olup olmadığını denetlemek için sorgu **olduğu\_Aracısı\_etkin** sütununda **sys.databases** Katalog görünümü. En son açılan sorgu penceresinde aşağıdaki komutu yürütün.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Hizmet Aracısı durumu sorgulama](real-time-web-applications-with-signalr/_static/image25.png "hizmet Aracısı durumu sorgulama")

    *Hizmet Aracısı durumu sorgulama*
8. Varsa değerini **olduğu\_Aracısı\_etkin** veritabanı sütunudur &quot;0&quot;, etkinleştirmek için aşağıdaki komutu kullanın. Değiştirin **&lt;YOUR-veritabanı&gt;** veritabanını oluştururken ayarladığınız adıyla (örn: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Hizmet Aracısı etkinleştirme](real-time-web-applications-with-signalr/_static/image26.png "hizmet Aracısı etkinleştirme")

    *Hizmet Aracısı etkinleştirme*

    > [!NOTE]
    > Bu sorgu görünürse emin olmak için kilitlenme DB'ye bağlı hiç uygulama yok.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Görev 3: SignalR uygulamayı yapılandırma

Bu görevde, yapılandıracağınız **Geek test** SQL Server devre kartına bağlanmak için. İlk ekleyeceksiniz **SignalR.SqlServer** NuGet paketi kümesi bağlantı dizesi devre kartı veritabanınıza.

1. Açık **Paket Yöneticisi Konsolu** gelen **Araçları** | **kitaplık Paket Yöneticisi**. Emin olun **GeekQuiz** projenin içinde seçili **varsayılan proje** aşağı açılan listesi. Yüklemek için aşağıdaki komutu yazın **Microsoft.AspNet.SignalR.SqlServer** NuGet paketi.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Proje için önceki adımı ancak bu kez tekrarlayın **GeekQuiz2**.
3. SQL Server devre kartına yapılandırmak için açık **Startup.cs** dosya **GeekQuiz** proje ve aşağıdaki kodu ekleyin **yapılandırma** yöntemi. Değiştirin **&lt;YOUR-veritabanı&gt;** ile SQL Server devre kartına oluştururken, veritabanı adı. Bu adım için yineleyin **GeekQuiz2** proje.

    (Kod parçacığını - *RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Her iki proje SQL Server devre kartına kullanmak üzere yapılandırılmış, basın **F5** aynı anda çalıştırmak için.
5. Yeniden **Visual Studio** iki örneği başlatacak **Geek test** farklı bağlantı noktaları. Sol tarafta, ekranınızın sağ taraftaki diğer tarayıcılardan birini sabitleme ve kimlik bilgilerinizle oturum açın. Meraklısına Notlar sayfanın sol tarafta tutun ve Git **istatistikleri** pageın doğru tarayıcı.
6. Sol tarayıcıda soruyu yanıtlayarak başlatın. Bu kez, **istatistikleri** sayfası devre kartına sayesinde güncelleştirildi. Uygulamalar arasında geçiş yapma (**istatistikleri** solda, sunulmuştur ve **Meraklısına Notlar** sağ tarafta olduğundan) ve test için her iki örnek çalıştığını doğrulamak için yineleyin. Devre kartı olarak hizmet veren bir *paylaşımlı önbellek* bağlı her sunucu ve her sunucu için iletilerin iletileri bağlı istemcilere dağıtmak için kendi yerel önbellekte depolar.
7. Visual Studio'ya geri dönün ve hata ayıklamayı durdurun.
8. SQL Server devre kartı bileşeni, belirtilen veritabanında gerekli tabloları otomatik olarak oluşturur. İçinde **SQL Server Nesne Gezgini** panelinde, devre kartı için oluşturduğunuz veritabanına açın (örn: SignalR) ve alt tablolar'ı genişletin. Aşağıdaki tablolarda görmeniz gerekir:

    ![Devre kartına tabloları oluşturulan](real-time-web-applications-with-signalr/_static/image27.png)

    *Devre kartına tabloları oluşturulan*
9. Sağ **SignalR.Messages\_0** tablosunu seçip **görünüm verilerini**.

    ![SignalR devre kartına ileti tabloyu görüntüle](real-time-web-applications-with-signalr/_static/image28.png)

    *SignalR devre kartına ileti tabloyu görüntüle*
10. Gönderilen farklı iletileri görebilirsiniz **Hub** trivia soruyu yanıtlayarak olduğunda. Devre kartına bağlı herhangi bir örneğe bu iletileri dağıtır.

    ![Devre kartına ileti tablo](real-time-web-applications-with-signalr/_static/image29.png)

    *Devre kartına ileti tablo*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuvarda nasıl ekleneceğini öğrendiniz **SignalR** uygulama ve gönderme bildirimlerinizi sunucudan kullanan, bağlı istemciler için **Hubs**. Kullanarak, uygulamanın ölçeğini genişletmek öğrendiniz ek olarak, bir *devre kartı* uygulamanız birden çok IIS örneğinde dağıtıldığında bileşeni.
