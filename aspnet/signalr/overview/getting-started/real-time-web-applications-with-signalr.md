---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Laboratuvar durum: SignalR ile gerçek zamanlı Web uygulamaları | Microsoft Docs'
author: rick-anderson
description: Gerçek zamanlı Web uygulamaları, sunucu tarafı, gerçek zamanlı olarak olduğu sürece bağlı istemcilere içerik anında iletme yeteneği özelliği. ASP.NET geliştiricilerinin ASP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878054"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratuvar durum: SignalR ile gerçek zamanlı Web uygulamaları
====================
Tarafından [Web Camps ekibi](https://twitter.com/webcamps)

[Kit eğitim Web Camps indirin](http://aka.ms/webcamps-training-kit)

> Gerçek zamanlı Web uygulamaları, sunucu tarafı, gerçek zamanlı olarak olduğu sürece bağlı istemcilere içerik anında iletme yeteneği özelliği. ASP.NET geliştiricilerinin **ASP.NET SignalR** kullandıkları uygulamalar için gerçek zamanlı web işlevselliği eklemek için bir kitaplıktır. Otomatik olarak verilen istemci ve sunucunun en iyi mevcut taşıma en iyi kullanılabilir taşıma seçme birkaç taşımaları yararlanır. Bunu yararlanan **WebSocket**, tarayıcı ve sunucu arasında çift yönlü iletişimi sağlayan HTML5 API.
> 
> **SignalR** ayrıca istemci RPC sunucusu yapmak için basit, üst düzey bir API sağlar (sunucu tarafı .NET kodundan müşterinizin tarayıcılarda JavaScript işlevleri çağırmak) ASP.NET uygulamanızı, aynı zamanda bağlantı yönetimi için yararlı kancaları ekleme bağlanma ve kesin olayları, gruplandırma bağlantıları ve yetkilendirme gibi.
> 
> **SignalR** bazı istemci ve sunucu arasında gerçek zamanlı iş yapmak için gerekli olan taşımalar üzerinden bir soyutlamadır. A **SignalR** bağlantı olarak HTTP başlatır ve sonra yükseltilen bir **WebSocket** bağlantı varsa. **WebSocket** için ideal taşıma **SignalR**, sunucu belleği en verimli kullanılmasını kolaylaştırır bu yana en düşük gecikme ve en temel özelliklerin varsa (istemci arasındaki tam çift yönlü iletişimi gibi ve sunucu için), ancak ayrıca en katı gereksinimlere sahiptir: **WebSocket** kullanılmasını gerektiriyor **Windows Server 2012** veya **Windows 8**, birlikte **.NET framework 4.5**. Bu gereksinimler karşılanmazsa **SignalR** bağlantısını yapmak için diğer taşımaları kullanmayı dener (gibi *Ajax uzun yoklama*).
> 
> **SignalR** API istemciler ve sunucular iletişim için iki modeli içerir: **kalıcı bağlantılar** ve **hub**. A **bağlantı** tek alıcı, gönderme gruplandırılmış veya yayın iletileri için basit bir uç noktasını temsil eder. A **Hub** olan bağlantı yöntemleri birbirine doğrudan çağırmak için istemci ve sunucu veren bir API inşa daha üst düzey bir ardışık düzen.
> 
> ![SignalR mimarisi](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Genel Bakış

<a id="Objectives"></a>
### <a name="objectives"></a>Amaçlar

Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:

- Bildirimleri sunucudan SignalR kullanarak istemciye gönderme.
- SignalR kullanarak uygulama ölçek **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:

- [Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya daha büyük

<a id="Setup"></a>
### <a name="setup"></a>Kurulum

Bu uygulamalı Laboratuvar alıştırmaları çalıştırmak için ortamınızı ayarlamanız gerekir.

1. Bir Windows Explorer penceresi açın ve Gözat Laboratuvar için 's **kaynak** klasör.
2. Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırmanıza ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemi başlatmak için.
3. Kullanıcı Hesabı Denetimi iletişim kutusu gösterilirse, devam etmek için eylemi onaylayın.

> [!NOTE]
> Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların işaretli olduğundan emin olun.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Kod parçacıkları

Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz. Size kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.

> [!NOTE]
> Her alıştırma bulunan Başlangıç bir çözüm tarafından eşlik **başlamak** diğer bağımsız olarak her alıştırma izlemenizi sağlar alıştırmanın klasör. Lütfen bir alıştırma sırasında eklenen kod parçacıkları çözümleri başlangıç bunlardan eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın. Bir alıştırma için kaynak kodunu içinde da bulacaksınız bir **son** karşılık gelen alıştırmada adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözümü içeren klasör. Uygulamalı bu Laboratuvar çalışırken Ek Yardım gerekirse, bu çözümlerin kılavuz olarak kullanabilirsiniz.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Alıştırmaları

Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:

1. [SignalR kullanarak gerçek zamanlı verileri ile çalışma](#Exercise1)
2. [SQL Server kullanarak ölçeklendirme](#Exercise2)

Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**

> [!NOTE]
> Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir. Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler. Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu. Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Alıştırma 1: SignalR kullanarak gerçek zamanlı verileri ile çalışma

Sohbet genellikle bir örnek olarak kullanılır, ancak bir bütün yapabileceğiniz çok daha fazla ile gerçek zamanlı Web işlevselliği. Bir kullanıcı yeni veri ya da sayfa Implements Ajax yeni verileri almak için uzun görmek için bir web sayfasını yeniler dilediğiniz zaman SignalR kullanabilirsiniz.

SignalR destekleyen **sunucu itme** veya **yayın** işlevselliği; bağlantı yönetimi otomatik olarak yönetir. Klasik HTTP bağlantılarında istemci-sunucu iletişimi için bağlantı her istek için yeniden kurulur kurulmaz, ancak SignalR istemci ve sunucu arasında kalıcı bir bağlantı sağlar. Uzaktan yordam çağrılarını (RPC) kullanarak tarayıcıda istemci kodu için sunucu kodu çağırır SignalR öğesinde yerine istek-yanıt modeli bugün biliyoruz.

Bu alıştırmada, yapılandıracağınız **günlük test** uygulama SignalR tüm sayfayı yenilemek için gerek kalmadan güncelleştirilmiş ölçümlerle istatistikleri panoyu görüntülemek için kullanın.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Görev 1 – günlük test istatistikleri sayfası keşfetme

Bu görevde, üzerinden uygulama gidin ve İstatistikler sayfası nasıl gösterilen doğrulayın ve bilgileri şekilde nasıl artırabilir güncelleştirilir.

1. Açık **için Visual Studio Express 2013 Web** ve açın **GeekQuiz.sln** çözüm bulunan **Source\Ex1 WorkingWithRealTimeData\Begin** klasör.
2. Tuşuna **F5** çözümü çalıştırın. **Oturum** sayfasını tarayıcıda görünmelidir.

    ![Çözümü çalıştırırken](real-time-web-applications-with-signalr/_static/image2.png "çözümünü çalıştırma")

    *Çözüm çalıştırma*
3. Tıklatın **kaydetmek** uygulamasında yeni bir kullanıcı oluşturmak için sayfanın sağ üst köşesinde içinde.

    ![Bağlantıyı kaydetmek](real-time-web-applications-with-signalr/_static/image3.png "kayıt bağlantı")

    *Bağlantı kaydetme*
4. İçinde **kaydetmek** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetmek**.

    ![Bir kullanıcı kaydetme](real-time-web-applications-with-signalr/_static/image4.png "kullanıcı kaydetme")

    *Bir kullanıcı kaydetme*
5. Yeni hesap uygulama kaydeder ve kullanıcı kimlik doğrulaması ve geri ilk test soru gösteren giriş sayfasına yeniden yönlendirildi.
6. Açık **istatistikleri** sayfasında yeni bir pencerede ve put **giriş** sayfa ve **istatistikleri** sayfasında yan yana.

    ![Yan yana windows](real-time-web-applications-with-signalr/_static/image5.png "yan tarafında windows tarafından")

    *Yan yana windows*
7. İçinde **giriş** sayfasında, seçeneklerden birine tıklayarak soruyu yanıtlayın.

    ![Bir soruyu yanıtlarken](real-time-web-applications-with-signalr/_static/image6.png "bir soru yanıtlama")

    *Bir soru yanıtlama*
8. Düğmeleri birini tıkladıktan sonra yanıt görüntülenmesi gerekir.

    ![Sorusunu yanıtladığınıza doğru](real-time-web-applications-with-signalr/_static/image7.png "sorusunu yanıtladığınıza doğru")

    *Doğru yanıtların soru*
9. İstatistikleri sayfasında sağlanan bilgileri güncel olduğuna dikkat edin. Güncelleştirilmiş sonuçları görmek için sayfayı yenileyin.

    ![İstatistikleri sayfası](real-time-web-applications-with-signalr/_static/image8.png "istatistikleri sayfası")

    *İstatistikleri sayfası*
10. Visual Studio'ya geri dönün ve hata ayıklamayı durdurun.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Görev 2 – çevrimiçi grafikleri göstermeyi günlük test için ekleme SignalR

Bu görevde, SignalR çözüme eklemek ve yeni bir yanıtı sunucuya gönderildiğinde güncelleştirmelerini istemcilere otomatik olarak gönder.

1. Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
2. İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu yürütün:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![SignalR paket yükleme](real-time-web-applications-with-signalr/_static/image9.png "SignalR paket yükleme")

    *SignalR paket yükleme*

   > [!NOTE]
   > Yüklerken **SignalR** NuGet paketleri sürümünden 2.0.2 yepyeni bir MVC 5 uygulaması, el ile güncelleştirmeniz gerekecektir **OWIN** sürüm 2.0.1 paketlere (veya sonrası) SignalR yüklemeden önce. Bunu yapmak için aşağıdaki komut dosyasında yürütebilir **Paket Yöneticisi Konsolu**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > Gelecekteki bir SignalR sürümünde, OWIN bağımlılıkları otomatik olarak güncelleştirilir.
3. İçinde **Çözüm Gezgini**, genişletin **betikleri** klasörü ve dikkat edin, SignalR *js* dosyaları çözüme eklendi.

    ![SignalR JavaScript başvuran](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript başvurur")

    *SignalR JavaScript başvuruları*
4. İçinde **Çözüm Gezgini**, sağ **GeekQuiz** proje, select **Ekle** | **yeni klasör**ve adlandırın **Hub**.
5. Sağ **hub** klasörü ve select **Ekle | Yeni öğe**.

    ![Yeni Öğe Ekle](real-time-web-applications-with-signalr/_static/image11.png "Yeni Öğe Ekle")

    *Yeni Öğe Ekle*
6. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Visual C# | Web | SignalR** sol bölmesinde, düğümdeki **SignalR hub'ı sınıfı (v2)** center bölmesinden dosya adı **StatisticsHub.cs** tıklatıp **Ekle**.

    ![Ekle yeni öğe iletişim kutusu](real-time-web-applications-with-signalr/_static/image12.png "Ekle yeni öğe iletişim kutusu")

    *Yeni Öğe Ekle iletişim kutusu*
7. Kodla **StatisticsHub** aşağıdaki kodla sınıfı.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Açık **haline** ve sonunda aşağıdaki satırı ekleyin **yapılandırma** yöntemi.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Açık **StatisticsService.cs** içinde sayfa **Hizmetleri** klasörü ve aşağıdaki yönergeleri kullanarak.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Güncelleştirmelerin bağlı istemciler bildirmek için önce aldığınız bir **bağlamı** geçerli bağlantı için nesne. **Hub** nesnesi tek bir istemci veya yayın için tüm bağlı istemcileri iletileri göndermek için yöntemler içerir. Aşağıdaki yöntemi ekleyin **StatisticsService** istatistik verileri yayınlamak için sınıf.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Yukarıdaki kod istemcide bir işlevi çağırmak için bir rastgele yöntem adı kullanıyorsanız (yani: *updateStatistics*). Yöntem adı IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelir dinamik bir nesne olarak yorumlanır. İfade, çalışma zamanında değerlendirilir. Yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değerlerini istemciye gönderir. İstemci adı ile eşleşen bir yöntemi varsa, bu yöntem çağrılır ve parametre değerleri için geçirilir. Eşleşen bir yöntem istemcide bulunursa, herhangi bir hata oluştu. Daha fazla bilgi için bkz [ASP.NET SignalR hub'ları API Kılavuzu](../guide-to-the-api/hubs-api-guide-server.md).
11. Açık **TriviaController.cs** içinde sayfa **denetleyicileri** klasörü ve aşağıdaki yönergeleri kullanarak.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Aşağıdaki vurgulanmış kodu ekleyin **Post** eylem yöntemi.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Açık **Statistics.cshtml** içinde sayfa **görünümleri | Giriş** klasör. Bulun **betikleri** bölümünde ve aşağıdaki komut dosyası başvuruları bölümün başında ekleyin.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Visual Studio projenizi SignalR ve diğer betik kitaplıkları eklediğinizde, Paket Yöneticisi SignalR komut dosyasının bu konuda gösterilen sürümden daha yeni bir sürümünü yükleyebilirsiniz. Komut başvurusu, kodunuzda projenizde yüklü kod kitaplığı sürümü eşleştiğinden emin olun.
14. SignalR hub'ına istemci bağlanma ve hub'ından yeni bir ileti alındığında istatistik verileri güncelleştirmek için aşağıdaki vurgulanmış kodu ekleyin.

    (Kod parçacığını - *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    Bu kodda bir Hub Proxy oluşturuyor ve sunucu tarafından gönderilen iletileri dinlemek için bir olay işleyicisi kaydediliyor. Bu durumda, üzerinden gönderilen iletiler için dinleme *updateStatistics* yöntemi.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Görev 3 – çözümünü çalıştırma

Bu görevde, otomatik olarak yeni bir soru yanıtlama sonra SignalR kullanarak, istatistikleri görünüm güncelleştirildiğini doğrulamak için çözüm çalışır.

1. Tuşuna **F5** çözümü çalıştırın.

    > [!NOTE]
    > Görev 1'de oluşturduğunuz kullanıcı oturum henüz uygulamada oturum açtığı, oturum.
2. Açık **istatistikleri** sayfasında yeni bir pencerede ve put **giriş** sayfa ve **istatistikleri** görev 1'de yaptığınız gibi yan yana sayfa.
3. İçinde **giriş** sayfasında, seçeneklerden birine tıklayarak soruyu yanıtlayın.

    ![Başka bir soru yanıtlama](real-time-web-applications-with-signalr/_static/image13.png "başka bir soru yanıtlama")

    *Başka bir soru yanıtlama*
4. Düğmeleri birini tıkladıktan sonra yanıt görüntülenmesi gerekir. Tüm sayfayı yenilemek için gerek kalmadan güncelleştirilmiş bilgileri sorusu yanıtlama sonra sayfada istatistik bilgileri otomatik olarak güncelleştirilir dikkat edin.

    ![İstatistikleri sayfa yenilenir yanıt sonra](real-time-web-applications-with-signalr/_static/image14.png "istatistikleri sayfa yenilenir yanıt sonra")

    *Yanıt sonra yenilenmesini istatistikleri sayfası*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Alıştırma 2: SQL Server kullanarak ölçeklendirme

Bir web uygulaması ölçekleme sırasında genellikle arasından seçim yapabilirsiniz *ölçeklendirmeyi* ve *ölçek genişletme* seçenekleri. *Ölçeği artırma* daha büyük bir sunucuya daha fazla kaynaklar (CPU, RAM, vb.) kullanarak anlamına gelir *ölçeğini* yükünü işlemek için daha fazla sunucu anlamına gelir. İstemciler farklı sunuculara yönlendirilen sorun ikinci olmasıdır. Bir sunucuya bağlı bir istemci, başka bir sunucudan gönderilen iletileri almaz.

Adlı bir bileşen kullanarak bu sorunları çözebilir *devre kartı*, sunucular arasında iletilerini iletecek şekilde. Etkin bir devre kartı ile her uygulama örneği devre kartına ileti gönderir ve bunları diğer uygulama örnekleri devre kartı iletir.

Şu anda backplanes SignalR için üç tür vardır:

- **Windows Azure hizmet veri yolu**. Hizmet veri yolu geniş eşleşmiş iletileri göndermek bileşenleri izin veren bir ileti altyapısıdır.
- **SQL Server**. SQL Server devre kartına ileti SQL tablolara yazar. Devre kartına verimli ileti için hizmet Aracısı kullanır. Service Broker etkin değilse ancak, bu da çalışır.
- **Redis**. Redis bir bellek içi anahtar-değer deposudur. Redis ileti göndermek için bir yayımlama/abonelik ("pub/alt") desen destekler.

Her ileti bir ileti veri yolu gönderilir. Bir ileti veri yoluna uygulayan [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) Yayımla ve abone bir Özet sağlar arabirimi. Varsayılan değiştirerek backplanes iş **IMessageBus** o devre kartı için tasarlanmış bir veri yolu ile.

Her sunucu örneği devre kartı veri yolu üzerinden bağlanır. Bir ileti gönderildiğinde devre kartına gider ve isteğe bağlı olarak devre kartı her sunucuya gönderir. Bir sunucu kartından bir ileti aldığında, iletiyi yerel önbelleğinde saklar. Sunucu, bunun yerel önbelleğinden istemcilere iletileri sonra sunar.

SignalR devre kartı, bunun işleyişi hakkında daha fazla bilgi için [makale](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Burada bir devre kartı bir ayak bağı olabilir bazı senaryolar vardır. Bazı tipik SignalR senaryolar şunlardır:
> 
> - [Sunucu yayın](tutorial-server-broadcast-with-signalr.md) (örn., borsa): Backplanes iletileri gönderilir oranı sunucu denetimleri olduğundan bu senaryo için iyi çalışır.
> - [İstemci istemci](tutorial-getting-started-with-signalr.md) (örn., sohbet): istemci sayısı ileti sayısını ölçeklendirir Bu senaryoda devre kartı bir performans sorunu olabilir; diğer bir deyişle, iletiler oranını büyürse orantılı olarak daha fazla istemci katılma.
> - [Yüksek yoğunlukta gerçek zamanlı](tutorial-high-frequency-realtime-with-signalr.md) (örneğin, gerçek zamanlı oyun): bir devre kartı bu senaryo için önerilmez.


Bu alıştırmada, kullanacağınız **SQL Server** iletileri üzerinden dağıtmak için **günlük test** uygulama. Bu görevler yapılandırmayı ayarlamak için ancak tam etkisi alabilmek için öğrenmek için tek test makinesi üzerinde çalışır, iki veya daha fazla sunucu SignalR uygulama dağıtmanız gerekir. SQL Server sunuculardan birinde ya da ayrılmış ayrı bir sunucuya yüklemeniz gerekir.

![SQL Server diyagramı kullanarak ölçek](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Görev 1 - senaryo anlama

Bu görevde, 2 örneklerini çalışacak **günlük test** yerel makinenizde örnekleri birden çok IIS benzetimini yapma. Bu senaryoda, bir uygulama üzerinde Trivia sorulara yanıt verilmesi, güncelleştirme ikinci örneği İstatistikler sayfasında bildirilmez. Uygulamanız birden çok örneği üzerinde dağıtıldığı bir ortamda bu benzetim benzer ve onlarla iletişim kurmak için bir yük dengeleyici kullanma.

1. Açık **Begin.sln** çözüm bulunan **kaynak/Ex2-ScalingOutWithSQLServer/başlangıç** klasör. Yüklenen sonra üzerinde fark edeceksiniz **Sunucu Gezgini** yapıları ancak farklı adlar çözümü aynı olan iki proje yok. Bu, yerel makinenizde iki örneğini aynı uygulamayı çalıştıran benzetimini yapacaksınız.

    ![Günlük test 2 örneklerini benzetimi çözüm başlamak](real-time-web-applications-with-signalr/_static/image16.png "başlamak çözüm günlük test 2 örneklerini benzetimini yapma")

    *Günlük test 2 örneklerini benzetimi çözüm başlayın*
2. Çözüm düğümüne sağ tıklayıp seçerek çözüm özellikler sayfasını açmak **özellikleri**. Altında **başlangıç projesi**seçin **birden fazla başlangıç projesi** değiştirip **eylem** her iki proje için değer *Başlat*.

    ![Birden çok proje başlangıç](real-time-web-applications-with-signalr/_static/image17.png "birden çok proje başlatılıyor")

    *Birden çok proje başlatılıyor*
3. Tuşuna **F5** çözümü çalıştırın. İki örnek uygulama başlatacak **günlük test** aynı uygulamayı birden çok örneğini farklı bağlantı noktaları benzetimini yapma. Sol taraftaki ekranınızın sağ diğer tarayıcılardan birini sabitleyin. Kimlik bilgileriyle oturum açın veya yeni bir kullanıcı kaydı. Oturum açtıktan sonra solda Trivia sayfa tutmak ve Git **istatistikleri** sayfasını tarayıcıda sağdaki.

    ![Günlük test yan yana](real-time-web-applications-with-signalr/_static/image18.png)

    *Günlük test yan yana*

    ![Günlük test farklı bağlantı noktaları](real-time-web-applications-with-signalr/_static/image19.png)

    *Günlük test farklı bağlantı noktaları*
4. Soru yanıtlama sol tarayıcıda başlatmak ve göreceksiniz **istatistikleri** sayfasını sağ tarayıcıda değil güncelleştiriliyor. Bunun nedeni, **SignalR** kullanan bir yerel önbelleğe iletilerini istemcileri dağıtmak için ve bu senaryo birden çok örneği benzetimini yapma, bu nedenle önbellek bunlar arasında paylaşılmaz. Olduğunu doğrulayabilirsiniz **SignalR** aynı adımları test ancak tek bir uygulama kullanarak çalışma. Aşağıdaki görevleri örneklerinde iletileri çoğaltılmasını devre kartı yapılandırır.
5. Visual Studio'ya geri dönün ve hata ayıklamayı durdurun.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Görev 2 – SQL Server devre kartı oluşturma

Bu görevde için devre kartı olarak davranacak bir veritabanı oluşturacak **günlük test** uygulama. Kullanacağınız **SQL Server Nesne Gezgini** sunucunuza gidin ve veritabanını başlatılamadı. Ayrıca, etkinleştirecek **hizmet Aracısı**.

1. İçinde **Visual Studio**açın menü **Görünüm** seçip **SQL Server Nesne Gezgini**.
2. Sağ tıklayarak, yerel veritabanı örneğine bağlanın **SQL Server** düğümü ve seçerek **SQL Server Ekle...**  seçeneği.

    ![Bir SQL Server örneği ekleme](real-time-web-applications-with-signalr/_static/image20.png "bir SQL Server örneği ekleme")

    *Bir SQL Server örneği için SQL Server Nesne Gezgini ekleme*
3. Ayarlama **sunucu adı** için *(localdb) \v11.0* bırakıp **Windows kimlik doğrulaması** , kimlik doğrulama modu. Tıklatın **Bağlan** devam etmek için.

    ![Yerel veritabanına bağlanma](real-time-web-applications-with-signalr/_static/image21.png "yerel veritabanına bağlanma")

    *Yerel veritabanına bağlanma*
4. Yerel veritabanı örneğine bağlanan, SignalR için SQL Server devre kartı temsil edecek bir veritabanı oluşturmak gerekir. Bunu yapmak için sağ **veritabanları** düğümü ve select **yeni veritabanı ekleme**.

    ![Yeni bir veritabanı ekleme](real-time-web-applications-with-signalr/_static/image22.png "yeni bir veritabanı ekleme")

    *Yeni bir veritabanı ekleme*
5. Veritabanı adı ayarlamak *SignalR* tıklatıp **Tamam** oluşturabilirsiniz.

    ![SignalR veritabanı oluşturma](real-time-web-applications-with-signalr/_static/image23.png "SignalR veritabanı oluşturma")

    *SignalR veritabanı oluşturma*

    > [!NOTE]
    > Veritabanı için herhangi bir ad seçebilirsiniz.
6. Güncelleştirmeleri kartından daha verimli bir şekilde almak için veritabanı için hizmet Aracısı etkinleştirmek için önerilir. Hizmet Aracısı ileti ve SQL Server'da sıraya alma için yerel destek sağlar. Devre kartına de hizmet aracısı çalışır. Yeni bir sorgu seçin ve veritabanı sağ tıklayarak açın **yeni sorgu**.

    ![Yeni bir sorgu açma](real-time-web-applications-with-signalr/_static/image24.png "yeni bir sorgu açma")

    *Yeni bir sorgu açma*
7. Service Broker etkin olup olmadığını denetlemek için sorgu **olan\_Aracısı\_etkin** sütununda **sys.databases** Katalog görünümü. Aşağıdaki komut dosyasını son açılan sorgu penceresinde yürütün.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Hizmet Aracısı durumu sorgulama](real-time-web-applications-with-signalr/_static/image25.png "hizmet Aracısı durumu sorgulanıyor")

    *Hizmet Aracısı durumu sorgulanıyor*
8. Varsa değerini **olan\_Aracısı\_etkin** veritabanınızdaki sütundur &quot;0&quot;, etkinleştirmek için aşağıdaki komutu kullanın. Değiştir **&lt;YOUR veritabanı&gt;** veritabanı oluşturulurken ayarlanan adı (ör: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Hizmet Aracısı etkinleştirme](real-time-web-applications-with-signalr/_static/image26.png "hizmet Aracısı etkinleştirme")

    *Hizmet Aracısı etkinleştirme*

    > [!NOTE]
    > Bu sorgu görünürse kilitlenme, emin olmak için Veritabanına bağlı uygulama yok.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Görev 3 – SignalR uygulamayı yapılandırma

Bu görevde, yapılandıracağınız **günlük test** SQL Server devre kartına bağlanmak için. İlk ekleyecek **SignalR.SqlServer** NuGet paketi ve kümesi bağlantı dizesi devre kartı veritabanınızı.

1. Açık **Paket Yöneticisi Konsolu** gelen **Araçları** | **kitaplık Paket Yöneticisi**. Olduğundan emin olun **GeekQuiz** proje seçildiğinde, **varsayılan proje** aşağı açılan liste. Yüklemek için aşağıdaki komutu yazın **Microsoft.AspNet.SignalR.SqlServer** NuGet paketi.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Önceki adımda ancak bu kez projesi için tekrarlayın **GeekQuiz2**.
3. SQL Server devre kartı yapılandırmak için açın **haline** dosyasının **GeekQuiz** proje ve aşağıdaki kodu ekleyin **yapılandırma** yöntemi. Değiştir **&lt;YOUR veritabanı&gt;** SQL Server devre kartı oluştururken kullandığınız veritabanı adıyla. İçin bu adımı yineleyin **GeekQuiz2** projesi.

    (Kod parçacığını - *RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Her iki projeye de SQL Server devre kartı kullanacak şekilde yapılandırılır, basın **F5** aynı anda çalıştırmak için.
5. Yeniden **Visual Studio** iki örneğini başlatacak **günlük test** farklı bağlantı noktaları. Sol taraftaki ekranınızın sağ diğer tarayıcılardan birini sabitleme ve kimlik bilgileriyle oturum açın. Trivia sayfanın sol tarafta tutmak ve Git **istatistikleri** sayfasını sağ tarayıcı.
6. Sol tarayıcıda sorulara yanıt verilmesi başlatın. Bu süre, **istatistikleri** sayfa sayesinde devre kartı güncelleştirilir. Uygulamalar arasında geçiş yapma (**istatistikleri** sol tarafta, sunulmuştur ve **Trivia** sağ tarafta olan) ve test için iki örnek çalıştığını doğrulamak için yineleyin. Devre kartına görevi gören bir *önbellek paylaşılan* her bağlı sunucu ve her sunucu için iletilerinin iletileri bağlı istemcilere dağıtmak için kendi yerel önbelleğine depolar.
7. Visual Studio'ya geri dönün ve hata ayıklamayı durdurun.
8. SQL Server devre kartı bileşeni, belirtilen veritabanında gerekli tabloları otomatik olarak oluşturur. İçinde **SQL Server Nesne Gezgini** paneli, devre kartı için oluşturduğunuz veritabanını açın (örneğin: SignalR) ve onun tabloları genişletin. Aşağıdaki tablolarda görmeniz gerekir:

    ![Tabloları devre kartı oluşturulan](real-time-web-applications-with-signalr/_static/image27.png)

    *Tabloları devre kartı oluşturulan*
9. Sağ **SignalR.Messages\_0** tablo ve seçin **görünüm verilerini**.

    ![Görünüm SignalR devre kartı iletileri tablosu](real-time-web-applications-with-signalr/_static/image28.png)

    *Görünüm SignalR devre kartı iletileri tablosu*
10. Gönderilen farklı iletileri görebilirsiniz **Hub** trivia sorulara yanıt verilmesi olduğunda. Devre kartına bağlı herhangi bir örneğine bu iletiler dağıtır.

    ![Devre kartına ileti tablosunun](real-time-web-applications-with-signalr/_static/image29.png)

    *Devre kartına ileti tablosunun*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Özet

Bu uygulamalı laboratuar ortamında nasıl ekleneceği öğrendiniz **SignalR** , uygulama ve gönderme bildirimleri sunucudan kullanarak bağlı istemcilerinize **hub**. Uygulamanızı kullanarak ölçeklendirmek öğrendiniz ek olarak, bir *devre kartı* uygulamanız birden çok IIS örneğinde dağıtıldığında bileşeni.
