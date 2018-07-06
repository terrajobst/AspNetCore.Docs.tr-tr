---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Öğretici: SignalR 2 ile yüksek sıklıkta gerçek | Microsoft Docs'
author: pfletcher
description: Bu öğretici, ASP.NET SignalR, yüksek frekanslı Mesajlaşma işlevleri sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Yüksek frekanslı..., Mesajlaşma
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14ec1c8b4c034332afd8d3102a310fd3fb399d32
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829469"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Öğretici: SignalR 2 ile yüksek sıklıkta gerçek
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

[Projeyi yükle](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Bu öğreticide, yüksek frekanslı Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Yüksek frekanslı Mesajlaşma bu durumda sabit bir fiyat karşılığında gönderilen güncelleştirmeleri anlamına gelir; Bu uygulama söz konusu olduğunda, en fazla 10 saniyenin iletileri.
> 
> Bu öğreticide oluşturacağınız uygulama kullanıcıları sürükleyebilirsiniz bir şekil görüntüler. Diğer tüm bağlı tarayıcıların şekil konumunu Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde güncelleştirilecektir.
> 
> Bu öğreticide tanıtılan kavramları, gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamaları vardır.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Bu öğreticide Visual Studio 2012 kullanarak
> 
> 
> Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:
> 
> - Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.
> - Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).
> - Web Platformu Yükleyicisi'nde arama ve yükleme **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**. Bu SignalR sınıflar için Visual Studio şablonları gibi yükleyecek **Hub**.
> - Bazı şablonlar (gibi **OWIN başlangıç sınıfı**) kullanılabilir; olmayacaktır, bunlar için sınıf dosyası kullanın.
> 
> 
> ## <a name="tutorial-versions"></a>Öğretici sürümleri
> 
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, diğer tarayıcılarda gerçek zamanlı olarak bir nesnenin durumu paylaşan bir uygulamanın nasıl oluşturulacağını gösterir. Uygulama oluşturacağız MoveShape çağrılır. Kullanıcı sürükleyebilirsiniz bir HTML Div öğesi MoveShape sayfası görüntüler; Kullanıcı Div sürüklediğinde, yeni konumuna ardından eşleştirilecek şeklin konum güncelleştirmek için diğer tüm bağlı istemcileri söyleyecektir sunucuya gönderilir.

![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Bu öğreticide oluşturulan uygulama tarafından Damian Edwards bir tanıtım temel alır. Bu Tanıtım içeren bir video görülebilir [burada](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Öğreticiyi şekli sürüklediğiniz olarak çalıştırılan her olaydan SignalR iletileri göndermek nasıl yazılacağını gösteren tarafından başlatılır. Her bağlı istemci, her zaman bir ileti alındığında sonra yerel sürüm şeklin konumunu güncelleştirir.

Bu yöntemi kullanarak uygulama çalışır durumdayken olacaktır, böylece istemciler ve sunucu iletileri ile dolmasını ve performans düşmesine neden gönderilen ileti sayısı için üst sınır bu yana bu önerilen bir programlama modeli değil . İstemci üzerinde görüntülenen animasyonu da şekli anında her yöntemle taşındı ayrık yerine her yeni konuma sorunsuz taşıma olacaktır. Öğreticinin sonraki bölümlerde, iletileri istemci veya sunucu tarafından gönderilen en yüksek hızı kısıtlayan bir zamanlayıcı işlevinin nasıl oluşturulacağı ve şekli sorunsuz konumlar arasında taşıma gösterilecektir. Bu öğreticide oluşturduğunuz uygulamanın son sürümü yüklenebilir [kod Galerisi](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Bu öğreticide, aşağıdaki bölümleri içerir:

- [Önkoşulları](#prerequisites)
- [Projeyi oluşturmak ve SignalR ve JQuery.UI NuGet paketi ekleme](#createtheproject2013)
- [Temel uygulama oluşturma](#baseapp)
- [Uygulama başladığında hub'ı başlatma](#startup2013)
- [İstemci döngü Ekle](#clientloop)
- [Sunucu döngü Ekle](#serverloop)
- [İstemcide düzgün animasyon ekleme](#animation)
- [Başka bir adım](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, Visual Studio 2013 gerektirir.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Projeyi oluşturmak ve SignalR ve JQuery.UI NuGet paketi ekleme

Bu bölümde, projenin Visual Studio 2013'te oluşturacağız.

ASP.NET boş Web uygulaması oluşturma ve SignalR ve jQuery.UI kitaplıkları eklemek için Visual Studio 2013'ün aşağıdaki adımları kullanın:

1. Visual Studio'da ASP.NET Web uygulaması oluşturun.

    ![Web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. İçinde **yeni ASP.NET projesi** penceresinde bırakın **boş** tıklatın ve seçili **proje oluştur**.

    ![Boş web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | SignalR Hub sınıfı (v2)**. Sınıf adı **MoveShapeHub.cs** ve projeye ekleyin. Bu adımda oluşturulur **MoveShapeHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen bir bütünleştirilmiş kod başvuruları projeye ekler.

    > [!NOTE]
    > Tıklayarak bir projeye SignalR ekleyebilirsiniz **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve bir komutu çalıştırın:

    `install-package Microsoft.AspNet.SignalR`. 

    SignalR eklemek için konsolunu kullanırsanız, SignalR hub sınıfı SignalR ekledikten sonra ayrı bir adım olarak oluşturun.
4. Tıklayın **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**. Paket Yöneticisi penceresinde aşağıdaki komutu çalıştırın:

    `Install-Package jQuery.UI.Combined`

    Bu şeklin animasyon uygulamak için kullanacağınız jQuery kullanıcı Arabirimi kitaplığı yükler.
5. İçinde **Çözüm Gezgini** betikleri düğümünü genişletin. Betik kitaplıkları için jQuery jQueryUI ve SignalR projesinde görünür.

    ![Komut dosyası kitaplık başvuruları](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Temel uygulama oluşturma

Bu bölümde, Şekil konumunu sunucuya her fare hareketi olayını sırasında gönderir. bir tarayıcı uygulaması oluşturacağız. Alınan sunucu daha sonra diğer tüm bağlı istemcileri bu bilgileri yayınlar. Sonraki bölümlerde bu uygulamada şirket genişletin.

1. Zaten MoveShapeHub.cs sınıf oluşturduğunuz yapmadıysanız, **Çözüm Gezgini**, projeye sağ tıklayıp **Ekle**, **sınıfı...** . Sınıf adı **MoveShapeHub** tıklatıp **Ekle**.
2. Yeni bir kodu **MoveShapeHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub` Yukarıdaki sınıfı, bir SignalR hub'ı uygulaması. Olarak [SignalR ile çalışmaya başlama](tutorial-getting-started-with-signalr.md) Öğreticisi, hub'ına istemcileri doğrudan çağıran bir yöntem. Bu durumda, istemci içeren yeni bir nesne gönderir sonra diğer tüm bağlı istemcileri yayımladınız sunucunun şekle X ve Y koordinatları. SignalR, otomatik olarak JSON'ı kullanarak bu nesneyi serileştirmek.

    İstemciye gönderilecek nesne (`ShapeModel`) şekil konumunu saklamak için üyeleri içerir. Belirli bir istemci, kendi veri gönderilmez, böylece nesnenin sunucuda hangi istemci verilerinin depolandığını, izlemek için bir üyesi de içerir. Bu üyeyi kullanan `JsonIgnore` , serileştirilmiş ve istemciye gönderilen tutmak için özniteliği.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Uygulama başladığında hub'ı başlatma

1. Uygulama başladığında ardından, biz hub'ına eşleme ayarlarsınız. SignalR 2'de bu çağıracak bir OWIN başlangıç sınıfı ekleyerek yapılır `MapSignalR` olduğunda Başlangıç sınıfı `Configuration` yöntemi OWIN başladığında yürütülür. Sınıf için OWIN'ın başlangıç eklenir oluşturabilirsiniz.computercube `OwinStartup` derleme özniteliği.

    İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | OWIN başlangıç sınıfı**. Sınıf adı *başlangıç* tıklatıp **Tamam**.
2. Startup.cs içeriğini aşağıdaki gibi değiştirin:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>İstemci ekleme

1. Ardından, istemci ekleyeceğiz. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Html sayfası**. Sayfayı adlandırın **Default.html** tıklatıp **Ekle**.
2. İçinde **Çözüm Gezgini**, yeni oluşturduğunuz sayfanın sağ tıklatıp **Başlangıç Sayfası Ayarla**.
3. Varsayılan HTML sayfasını aşağıdaki kod parçacığı ile değiştirin.

    > [!NOTE]
    > Komut dosyası projenize betikler klasörüne eklenen paketler eşleşme başvurduğunu doğrulayın.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Yukarıdaki HTML ve JavaScript kodunu adlı şekli kırmızı bir Div oluşturur, jQuery kitaplığını kullanarak şeklin sürükleyerek davranışını etkinleştirir ve şeklin `drag` şeklin konum sunucuya göndermek için olay.
4. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>İstemci döngü Ekle

Her fare hareketi olayını şeklinizde konumunu gönderme gerekli olmayan miktarda ağ trafiği oluşturur, istemciden gelen iletileri kısıtlanabilir gerekir. Javascript kullanacağız `setInterval` sabit bir hızda sunucuya yeni konum bilgileri gönderen bir döngü ayarlamak için işlevi. Bu döngü bir "Oyun döngüsü", tüm işlevlerin bir oyun veya diğer benzetimi sürücüleri tekrar tekrar çağrılan bir işlevin çok basit bir gösterimidir.

1. İstemci kodu HTML sayfasındaki, aşağıdaki kod parçacığı eşleşecek şekilde güncelleştirin.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Yukarıdaki güncelleştirme ekler `updateServerModel` işlevi çağrılan bir sabit sıklığı temel. Bu işlev, sunucuya konum verileri gönderir her `moved` bayrağı, yeni konum verileri göndermek için olduğunu gösterir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir. Animasyon, sunucuya gönderilen ileti sayısını kısıtlanacak olduğundan, önceki bölümdeki kesintisiz olarak görünmez.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Sunucu döngü Ekle

Geçerli uygulamada, sunucudan istemciye gönderilen iletileri alındıkları sıklıkta gönderilir. İstemcide görülen benzer bir sorun gösterir; iletiler daha sık gerekli olan ve bağlantı sonucunda yayılmamış olabilir gönderilebilir. Bu bölümde, giden iletiler oranını kısıtlar bir zamanlayıcı uygulamak için sunucu güncelleştirme işlemi açıklanmaktadır.

1. Öğesinin içeriğini değiştirin `MoveShapeHub.cs` aşağıdaki kod parçacığı ile.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Yukarıdaki kod eklemek için istemci genişletir `Broadcaster` giden kısıtlar sınıfı iletileri kullanarak `Timer` .NET Framework sınıfı.

    Hub geçici olduğundan (her zaman gerekli oluşturulduktan), `Broadcaster` tekil olarak oluşturulur. Bu, Zamanlayıcıyı başlatılmadan önce ilk hub örneği tamamen oluşturulduktan gerekli kadar oluşturulduktan erteleneceği yavaş başlatma (.NET 4'te sunulmuştur) kullanılır.

    İstemcilerin çağrısı `UpdateShape` işlevi ardından hub dışında taşınmış `UpdateModel` yöntemi, böylece onun artık hemen gelen mesajların alındığı zaman çağrılır. Bunun yerine, istemciler için iletileri, saniye başına 25 çağrılarının bir hızda gönderilecek tarafından yönetilen `_broadcastLoop` içinden Zamanlayıcı `Broadcaster` sınıfı.

    Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıfı gerekiyor şu anda işletim hub'ına bir başvuru almak (`_hubContext`) kullanarak `GlobalHost`.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir. Önceki bölümde tarayıcınızda görünür bir farkı olmayacak, ancak istemciye gönderilen ileti sayısını kısıtlanır.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>İstemcide düzgün animasyon ekleme

Uygulama neredeyse tamamlandı, ancak biz yanıt olarak sunucu ileti taşınırken bir daha fazla geliştirme istemcide şeklin halindeki yapabilir. Sunucu tarafından verilen yeni bir konuma şekil konumunu ayarlamak yerine JQuery kullanıcı Arabirimi kitaplığın kullanacağız `animate` şekli sorunsuz geçerli ve yeni konumu arasında taşımak için işlevi.

1. İstemci güncelleştirme `updateShape` yöntemi aramak için aşağıdaki vurgulanmış kodu ister:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Yukarıdaki kod şekli eski konumundan (Bu durumda, 100 milisaniye) animasyon aralığı boyunca sunucu tarafından verilen yeni bir tane taşır. Yeni animasyon başlatılmadan önce şekli üzerinde çalışan herhangi bir önceki animasyon temizlenir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir. Diğer pencere şeklinde hareketini hareketini gelen ileti başına bir kez ayarlanan yerine zaman içinde ilişkilendirilmiş daha az düzensiz görüntülenmesi gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Başka bir adım

Bu öğreticide, istemciler ve sunucular arasında yüksek frekanslı iletiler gönderen bir SignalR uygulama programı öğrendiniz. Bu iletişim paradigma çevrimiçi oyunlar ve diğer simülasyonları gibi geliştirmek için kullanışlıdır [SignalR ile oluşturulan ShootR game](http://shootr.signalr.net).

Bu öğreticide oluşturduğunuz uygulamanın indirilebileceğini [kod Galerisi](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

SignalR geliştirme kavramları hakkında daha fazla bilgi edinmek için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Bir SignalR uygulamayı azure'a dağıtma hakkında kılavuz için bkz. [Azure App service'taki Web Apps ile SignalR kullanarak](../deployment/using-signalr-with-azure-web-sites.md). Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
