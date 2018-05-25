---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Öğretici: Yüksek sıklıkta gerçek zamanlı SignalR 2 ile | Microsoft Docs'
author: pfletcher
description: Bu öğretici yüksek sıklıkta Mesajlaşma işlevleri sağlamak için ASP.NET SignalR kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Yüksek içinde ileti sıklıkta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>Öğretici: Yüksek sıklıkta gerçek zamanlı SignalR 2 ile
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> Bu öğretici yüksek sıklıkta Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Yüksek yoğunlukta Mesajlaşma bu durumda sabit bir oranda gönderilen güncelleştirmeler anlamına gelir; Bu uygulama söz konusu olduğunda, 10'a kadar saniyenin iletileri.
> 
> Bu öğreticide oluşturacaksınız uygulama kullanıcıların sürükleyebilirsiniz bir şekli görüntüler. Şeklin konumunu diğer bağlı tarayıcılarda Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde güncelleştirilecektir.
> 
> Bu öğreticide sunulan kavramlar gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamalar vardır.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Bu öğretici ile Visual Studio 2012 kullanma
> 
> 
> Visual Studio 2012 bu öğreticiyle kullanmak için aşağıdakileri yapın:
> 
> - Güncelleştirme, [Paket Yöneticisi](http://docs.nuget.org/docs/start-here/installing-nuget) en son sürüme.
> - Yükleme [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx).
> - Web Platformu Yükleyicisi'nde arayın ve yükleyin **ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için**. Bu SignalR sınıfları için Visual Studio şablonları gibi yükleyecek **Hub**.
> - Bazı şablonlar (gibi **OWIN başlangıç sınıfı**); kullanılamaz bunlar için bunun yerine bir sınıf dosyası kullanın.
> 
> 
> ## <a name="tutorial-versions"></a>Eğitmen sürümleri
> 
> SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu öğretici bir nesnenin durumunu gerçek zamanlı diğer tarayıcılarda paylaşan bir uygulamasının nasıl oluşturulacağını gösterir. Oluşturacağız uygulama MoveShape adı verilir. MoveShape sayfa kullanıcı sürükleyebilirsiniz bir HTML Div öğesinin görüntülenir; Kullanıcı Div sürüklendiğinde yeni konumunu ardından eşleştirilecek şeklin konumunu güncelleştirmek için diğer tüm bağlı istemcileri söyleyecektir sunucusuna gönderilir.

![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Bu öğreticide oluşturduğunuz uygulama tarafından Damian Edirneli demo üzerinde temel alır. Bu Tanıtım içeren bir video görülebilir [burada](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Öğretici şekli sürüklenen gibi tetiklenen her olay SignalR iletileri göndermek nasıl gösteren tarafından başlatılır. Her bağlı istemci sonra her zaman bir ileti alındığında şekli yerel sürümünü konumunu güncelleştirir.

Uygulama bu yöntemi kullanarak çalışır, ancak olurdu bu yana, böylece istemciler ve sunucu iletileri ile çok sayıda ve performansının düşmesine neden gönderilen iletisi sayısı için üst sınır bu önerilen bir programlama modeli değil . İstemci üzerinde görüntülenen animasyon de kopuk şekli hemen her yöntemi tarafından taşındı yerine her yeni konuma sorunsuz taşıma olacaktır. Öğreticinin sonraki bölümlerde iletileri istemci veya sunucu tarafından gönderilme en yüksek hızı kısıtlayan bir zamanlayıcı işlevi oluşturma ve Şekil sorunsuz konumlar arasında taşıma gösterilmektedir. Bu öğreticide oluşturduğunuz uygulamanın son sürümü karşıdan yüklenebilir [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

Bu öğretici aşağıdaki bölümleri içerir:

- [Önkoşulları](#prerequisites)
- [Proje oluşturma ve SignalR ve JQuery.UI NuGet paketi ekleme](#createtheproject2013)
- [Temel uygulama oluşturma](#baseapp)
- [Uygulama başladığında hub başlatılıyor](#startup2013)
- [İstemci döngü ekleme](#clientloop)
- [Sunucu döngü ekleme](#serverloop)
- [İstemcide düzgün animasyon ekleme](#animation)
- [Başka bir adım](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide Visual Studio 2013 gerektirir.

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>Proje oluşturma ve SignalR ve JQuery.UI NuGet paketi ekleme

Bu bölümde, Visual Studio 2013 projesinde oluşturacağız.

ASP.NET boş Web uygulaması oluşturmak ve SignalR ve jQuery.UI kitaplıkları eklemek için Visual Studio 2013 aşağıdaki adımları kullanın:

1. Visual Studio'da ASP.NET Web uygulaması oluşturun.

    ![Web oluşturulamıyor](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. İçinde **yeni ASP.NET projesi** penceresinde, bırakın **boş** 'ı tıklatın ve seçili **proje oluştur**.

    ![Boş web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | SignalR hub'ı sınıfı (v2)**. Sınıf adını **MoveShapeHub.cs** ve bunu projeye ekleyin. Bu adım oluşturur **MoveShapeHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen derleme başvurularını projeye ekler.

    > [!NOTE]
    > Tıklayarak projeye SignalR ekleyebilirsiniz **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve bir komut çalıştırma:

    `install-package Microsoft.AspNet.SignalR`. 

    SignalR eklemek için konsol kullanırsanız, SignalR hub'ı sınıf SignalR ekledikten sonra ayrı bir adım olarak oluşturun.
4. Tıklatın **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**. Paket Yöneticisi penceresinde aşağıdaki komutu çalıştırın:

    `Install-Package jQuery.UI.Combined`

    Bu şeklin animasyon için kullanacağınız jQuery kullanıcı Arabirimi kitaplığı yükler.
5. İçinde **Çözüm Gezgini** betikleri düğümünü genişletin. JQuery, jQueryUI ve SignalR için komut dosyası kitaplıkları projede görünür.

    ![Komut dosyası Kitaplığı Başvurusu](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Temel uygulama oluşturma

Bu bölümde, Şekil konumunu sunucuya her fare hareketi olayını sırasında gönderir. bir tarayıcı uygulaması oluşturacağız. Alındığında sunucu daha sonra bu bilgileri diğer tüm bağlı istemcileri için yayınlar. Sonraki bölümlerde bu uygulamada üzerinde genişletin.

1. Zaten MoveShapeHub.cs sınıfı içinde oluşturmadıysanız, **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle**, **sınıfı...** . Sınıf adını **MoveShapeHub** tıklatıp **Ekle**.
2. Kod yeni değiştirin **MoveShapeHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub` Sınıfı yukarıdaki bir SignalR hub'ının bir uygulamasıdır. Olarak [SignalR ile çalışmaya başlama](tutorial-getting-started-with-signalr.md) öğretici, hub istemcileri doğrudan çağıran bir yöntem vardır. Bu durumda, istemci içeren yeni bir nesne gönderir ardından diğer tüm bağlı istemcilere yayımladınız sunucusuna şeklin X ve Y koordinatları. SignalR otomatik olarak JSON kullanarak bu nesneyi seri hale.

    İstemciye gönderilecek nesne (`ShapeModel`) şeklin konumunu depolamak için üye içeriyor. Belirtilen bir istemci, kendi veri gönderilmez böylece sunucu üzerindeki nesnesi sürümünü hangi istemci verilerinin depolanıyorsa, izlemek için üye de içerir. Bu üyeyi kullanan `JsonIgnore` serileştirilmiş ve istemciye gönderilen tutmak için öznitelik.

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>Uygulama başladığında hub başlatılıyor

1. Uygulama başladığında ardından, hub'ına eşlemesini yaparız. SignalR 2'de bu çağıracak bir OWIN başlangıç sınıfı ekleyerek yapılır `MapSignalR` zaman başlangıç sınıfı `Configuration` yöntemi OWIN başladığında yürütülebilir. Sınıfı için OWIN'in başlangıç eklenir kullanarak işlem `OwinStartup` derleme özniteliği.

    İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | OWIN başlangıç sınıfı**. Sınıf adını *başlangıç* tıklatıp **Tamam**.
2. Haline içeriğini aşağıdaki gibi değiştirin:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>İstemci ekleme

1. Ardından, istemci ekleyeceğiz. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Html sayfası**. Sayfa adı **Default.html** tıklatıp **Ekle**.
2. İçinde **Çözüm Gezgini**, az önce oluşturduğunuz sayfanın sağ tıklatın ve **Başlangıç Sayfası Ayarla**.
3. HTML sayfası varsayılan kodda aşağıdaki kod parçacığıyla değiştirin.

    > [!NOTE]
    > Betik betikler klasörüne projenizde eklenen paketler eşleşme başvurduğundan emin olun.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    Yukarıdaki HTML ve JavaScript kodu şekli adlı bir kırmızı Div oluşturur, jQuery kitaplığını kullanarak şeklin sürükleme davranışı etkinleştirir ve şeklin kullanır `drag` şeklin konumu sunucusuna göndermek için olay.
4. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>İstemci döngü ekleme

Şekil her fare hareketi olayını üzerindeki konumunu gönderme gerekli olmayan miktarda ağ trafiği oluşturacak olduğundan, istemciden gelen iletileri kısıtlanan gerekir. Javascript kullanacağız `setInterval` sabit bir oranda sunucuya yeni konum bilgilerini gönderir bir döngü ayarlamak için işlevi. Bu döngü, bir "Oyun döngü", oyun veya diğer benzetimi işlevselliğini tüm sürücüleri art arda çağrılan bir işlev çok basit bir gösterimidir.

1. Aşağıdaki kod parçacığını eşleşecek şekilde HTML sayfasındaki istemci kodunu güncelleştirin.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    Yukarıdaki güncelleştirme ekler `updateServerModel` sabit frekansında çağrılır işlevi. Bu işlev konum verileri sunucuya gönderdiği her `moved` bayrağı göndermek için yeni konum verileri olduğunu gösterir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir. Animasyon, sunucuya gönderilen ileti sayısını kısıtlanacak olduğundan, olduğu gibi önceki bölümde kesintisiz olarak görünmez.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Sunucu döngü ekleme

Geçerli uygulamada sunucudan istemciye gönderilen iletileri alınana kadar sık gider. İstemcide görülen benzer bir sorun gösterir; iletiler daha sık gerekli değildir ve bağlantı sonucunda taşan hale gelebilir gönderilebilir. Bu bölümde, giden iletiler oranını kısıtlar bir süreölçer uygulamak için sunucuyu güncelleştirmek açıklar.

1. Değiştir `MoveShapeHub.cs` aşağıdaki kod parçacığını ile.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Yukarıdaki kod eklemek için istemci genişletir `Broadcaster` giden kısıtlar sınıfı iletileri kullanarak `Timer` .NET framework sınıf.

    Hub geçici olduğundan (gerektiğinde her zaman oluşturulduğu), `Broadcaster` bir singleton olarak oluşturulur. Geç Başlatma (.NET 4'te tanıtılan) oluşturma, Zamanlayıcı başlamadan önce ilk hub örneği tamamen oluşturulduğundan emin olduktan gerektiğinde kadar erteleme kullanılır.

    İstemcilerin çağrısı `UpdateShape` işlevi sonra merkezin dışında taşınmış `UpdateModel` gelen iletileri hemen alınan her onun artık adlı şekilde yöntemi. Bunun yerine, iletileri istemcilere saniye başına 25 çağrı hızında gönderilir tarafından yönetilen `_broadcastLoop` Zamanlayıcı içinden `Broadcaster` sınıfı.

    Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıf gereken şu anda işletim hub için bir başvuru almak (`_hubContext`) kullanarak `GlobalHost`.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir. Önceki bölümde tarayıcıdan görünür bir fark olmayacak, ancak istemciye gönderilen ileti sayısını kısıtlanacak.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>İstemcide düzgün animasyon ekleme

Uygulama neredeyse tamamlandı, ancak yanıt olarak sunucu ileti taşınırken bir daha fazla geliştirme, istemcide şeklin hareketli vermiyoruz. Sunucu tarafından verilen yeni bir konuma şeklin konumunu ayarlamak yerine JQuery UI kitaplığın kullanacağız `animate` şekli sorunsuz geçerli ve yeni konumunu arasında taşımak için işlevi.

1. İstemcinin güncelleştirme `updateShape` aramak için yöntemi aşağıdaki vurgulanmış kodu ister:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    Yukarıdaki kod şekli eski konumdan animasyon aralığı (Bu durumda, 100 milisaniyede) seyri içinde sunucu tarafından verilen yeni bir taşır. Yeni animasyon başlamadan önce Şekil üzerinde çalışan herhangi bir önceki animasyon temizlenir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir. Diğer pencere şeklinde hareketini hareketini üzerinden gelen ileti başına bir kez ayarlanan yerine zaman ilişkilendirilir daha az düzensiz görüntülenmesi gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Başka bir adım

Bu öğreticide, istemciler ve sunucular arasında yüksek sıklıkta iletileri gönderen bir SignalR uygulama programı nasıl öğrendiniz. Bu iletişim çevrimiçi oyunlar ve diğer benzetimleri gibi geliştirmek için yararlı bir örnektir [SignalR ile oluşturulan ShootR oyun](http://shootr.signalr.net).

Bu öğreticide oluşturduğunuz tam uygulama yüklenebilir [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a).

SignalR geliştirme kavramları hakkında daha fazla bilgi edinmek için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR Project](http://signalr.net)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

Bir SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md). Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
