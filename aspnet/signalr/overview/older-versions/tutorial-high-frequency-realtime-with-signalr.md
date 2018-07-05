---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: SignalR ile yüksek sıklıkta gerçek zamanlı 1.x | Microsoft Docs
author: pfletcher
description: Bu öğretici, ASP.NET SignalR, yüksek frekanslı Mesajlaşma işlevleri sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Yüksek frekanslı..., Mesajlaşma
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: d9a5539d79d0b6396282af8036f2c6417f44ce23
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401140"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>SignalR ile yüksek sıklıkta gerçek zamanlı 1.x
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

> Bu öğretici, ASP.NET SignalR, yüksek frekanslı Mesajlaşma işlevleri sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Yüksek frekanslı Mesajlaşma bu durumda sabit bir fiyat karşılığında gönderilen güncelleştirmeleri anlamına gelir; Bu uygulama söz konusu olduğunda, en fazla 10 saniyenin iletileri.
> 
> Bu öğreticide oluşturacağınız uygulama kullanıcıları sürükleyebilirsiniz bir şekil görüntüler. Diğer tüm bağlı tarayıcıların şekil konumunu Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde güncelleştirilecektir.
> 
> Bu öğreticide tanıtılan kavramları, gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamaları vardır.
> 
> Öğreticiyi hakkındaki yorumları davetlidir. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, diğer tarayıcılarda gerçek zamanlı olarak bir nesnenin durumu paylaşan bir uygulamanın nasıl oluşturulacağını gösterir. Uygulama oluşturacağız MoveShape çağrılır. Kullanıcı sürükleyebilirsiniz bir HTML Div öğesi MoveShape sayfası görüntüler; Kullanıcı Div sürüklediğinde, yeni konumuna ardından eşleştirilecek şeklin konum güncelleştirmek için diğer tüm bağlı istemcileri söyleyecektir sunucuya gönderilir.

![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Bu öğreticide oluşturulan uygulama tarafından Damian Edwards bir tanıtım temel alır. Bu Tanıtım içeren bir video görülebilir [burada](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Öğreticiyi şekli sürüklediğiniz olarak çalıştırılan her olaydan SignalR iletileri göndermek nasıl yazılacağını gösteren tarafından başlatılır. Her bağlı istemci, her zaman bir ileti alındığında sonra yerel sürüm şeklin konumunu güncelleştirir.

Bu yöntemi kullanarak uygulama çalışır durumdayken olacaktır, böylece istemciler ve sunucu iletileri ile dolmasını ve performans düşmesine neden gönderilen ileti sayısı için üst sınır bu yana bu önerilen bir programlama modeli değil . İstemci üzerinde görüntülenen animasyonu da şekli anında her yöntemle taşındı ayrık yerine her yeni konuma sorunsuz taşıma olacaktır. Öğreticinin sonraki bölümlerde, iletileri istemci veya sunucu tarafından gönderilen en yüksek hızı kısıtlayan bir zamanlayıcı işlevinin nasıl oluşturulacağı ve şekli sorunsuz konumlar arasında taşıma gösterilecektir. Bu öğreticide oluşturduğunuz uygulamanın son sürümü yüklenebilir [kod Galerisi](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Bu öğreticide, aşağıdaki bölümleri içerir:

- [Önkoşulları](#prerequisites)
- [Proje oluşturma](#createtheproject)
- [ASP.NET SignalR ve JQuery.UI NuGet paketleri Ekle](#nugetpackages)
- [Temel uygulama oluşturma](#baseapp)
- [İstemci döngü Ekle](#clientloop)
- [Sunucu döngü Ekle](#serverloop)
- [İstemcide düzgün animasyon ekleme](#animation)
- [Başka bir adım](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, Visual Studio 2012 veya Visual Studio 2010 gerektirir. Visual Studio 2010 kullanıyorsanız, projenin .NET Framework 4.5 yerine .NET Framework 4 kullanın.

Visual Studio 2012 kullanıyorsanız, yüklemeniz önerilir [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650). Bu güncelleştirme yayımlama, yeni işlevsellik geliştirmeler gibi yeni özellikler içerir ve yeni şablonlar.

Visual Studio 2010 varsa, emin [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklenir.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Projeyi oluşturma

Bu bölümde, Visual Studio'da proje oluşturacağız.

1. Gelen **dosya** menüsünü tıklatın **yeni proje**.
2. İçinde **yeni proje** iletişim kutusunda **C#** altında **şablonları** seçip **Web**.
3. Seçin **ASP.NET boş Web uygulaması** şablon, proje adı *MoveShapeDemo*, tıklatıp **Tamam**.

    ![Yeni proje oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>SignalR ve JQuery.UI NuGet paketleri Ekle

Bir NuGet paketini yükleme yoluyla projeye SignalR işlevleri ekleyebilirsiniz. Bu öğreticide JQuery.UI paket sürüklenebilen ve animasyon şekle izin vermek için de kullanır.

1. Tıklayın **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.
2. Paket Yöneticisi'nde aşağıdaki komutu girin.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR paket bağımlılıkları olarak birkaç NuGet paketi yükler. Yükleme tamamlandığında tüm SignalR bir ASP.NET uygulamasında kullanmak için gerekli istemci ve sunucu bileşenlerini sahip.
3. JQuery ve JQuery.UI paketleri yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu girin.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Temel uygulama oluşturma

Bu bölümde, Şekil konumunu sunucuya her fare hareketi olayını sırasında gönderir. bir tarayıcı uygulaması oluşturacağız. Alınan sunucu daha sonra diğer tüm bağlı istemcileri bu bilgileri yayınlar. Sonraki bölümlerde bu uygulamada şirket genişletin.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp **Ekle**, **sınıfı...** . Sınıf adı **MoveShapeHub** tıklatıp **Ekle**.
2. Yeni bir kodu **MoveShapeHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub` Yukarıdaki sınıfı, bir SignalR hub'ı uygulaması. Olarak [SignalR ile çalışmaya başlama](index.md) Öğreticisi, hub'ına istemcileri doğrudan çağıran bir yöntem. Bu durumda, istemci içeren yeni bir nesne gönderir sonra diğer tüm bağlı istemcileri yayımladınız sunucunun şekle X ve Y koordinatları. SignalR, otomatik olarak JSON'ı kullanarak bu nesneyi serileştirmek.

    İstemciye gönderilecek nesne (`ShapeModel`) şekil konumunu saklamak için üyeleri içerir. Belirli bir istemci, kendi veri gönderilmez, böylece nesnenin sunucuda hangi istemci verilerinin depolandığını, izlemek için bir üyesi de içerir. Bu üyeyi kullanan `JsonIgnore` , serileştirilmiş ve istemciye gönderilen tutmak için özniteliği.
3. Ardından, uygulama başladığında size hub'ı ayarladınız ayarlarsınız. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Genel uygulama sınıfı**. Varsayılan adı kabul *genel* tıklatıp **Tamam**.

    ![Genel uygulama sınıfı Ekle](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Aşağıdaki `using` deyiminden sonra sağlanan **kullanarak** deyimlerinde Global.asax.cs sınıfı.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Aşağıdaki kod satırını ekleyin `Application_Start` genel sınıfının, SignalR için varsayılan yolu kaydetmek için yöntemi.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Global.asax dosyanız aşağıdaki gibi görünmelidir:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Ardından, istemci ekleyeceğiz. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Html sayfası**. Sayfa uygun bir ad verin (gibi **Default.html**) tıklayıp **Ekle**.
7. İçinde **Çözüm Gezgini**, yeni oluşturduğunuz sayfanın sağ tıklatıp **Başlangıç Sayfası Ayarla**.
8. Varsayılan HTML sayfasını aşağıdaki kod parçacığı ile değiştirin.

    > [!NOTE]
    > Komut dosyası projenize betikler klasörüne eklenen paketler eşleşme başvurduğunu doğrulayın. Visual Studio 2010'da, JQuery ve projeye eklenen SignalR sürüm numaralarını eşleşmeyebilir.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Yukarıdaki HTML ve JavaScript kodunu adlı şekli kırmızı bir Div oluşturur, jQuery kitaplığını kullanarak şeklin sürükleyerek davranışını etkinleştirir ve şeklin `drag` şeklin konum sunucuya göndermek için olay.
9. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>İstemci döngü Ekle

Her fare hareketi olayını şeklinizde konumunu gönderme gerekli olmayan miktarda ağ trafiği oluşturur, istemciden gelen iletileri kısıtlanabilir gerekir. Javascript kullanacağız `setInterval` sabit bir hızda sunucuya yeni konum bilgileri gönderen bir döngü ayarlamak için işlevi. Bu döngü bir "Oyun döngüsü", tüm işlevlerin bir oyun veya diğer benzetimi sürücüleri tekrar tekrar çağrılan bir işlevin çok basit bir gösterimidir.

1. İstemci kodu HTML sayfasındaki, aşağıdaki kod parçacığı eşleşecek şekilde güncelleştirin.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Yukarıdaki güncelleştirme ekler `updateServerModel` işlevi çağrılan bir sabit sıklığı temel. Bu işlev, sunucuya konum verileri gönderir her `moved` bayrağı, yeni konum verileri göndermek için olduğunu gösterir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir. Animasyon, sunucuya gönderilen ileti sayısını kısıtlanacak olduğundan, önceki bölümdeki kesintisiz olarak görünmez.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Sunucu döngü Ekle

Geçerli uygulamada, sunucudan istemciye gönderilen iletileri alındıkları sıklıkta gönderilir. İstemcide görülen benzer bir sorun gösterir; iletiler daha sık gerekli olan ve bağlantı sonucunda yayılmamış olabilir gönderilebilir. Bu bölümde, giden iletiler oranını kısıtlar bir zamanlayıcı uygulamak için sunucu güncelleştirme işlemi açıklanmaktadır.

1. Öğesinin içeriğini değiştirin `MoveShapeHub.cs` aşağıdaki kod parçacığı ile.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Yukarıdaki kod eklemek için istemci genişletir `Broadcaster` giden kısıtlar sınıfı iletileri kullanarak `Timer` .NET Framework sınıfı.

    Hub geçici olduğundan (her zaman gerekli oluşturulduktan), `Broadcaster` tekil olarak oluşturulur. Bu, Zamanlayıcıyı başlatılmadan önce ilk hub örneği tamamen oluşturulduktan gerekli kadar oluşturulduktan erteleneceği yavaş başlatma (.NET 4'te sunulmuştur) kullanılır.

    İstemcilerin çağrısı `UpdateShape` işlevi ardından hub dışında taşınmış `UpdateModel` yöntemi, böylece onun artık hemen gelen mesajların alındığı zaman çağrılır. Bunun yerine, istemciler için iletileri, saniye başına 25 çağrılarının bir hızda gönderilecek tarafından yönetilen `_broadcastLoop` içinden Zamanlayıcı `Broadcaster` sınıfı.

    Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıfı gerekiyor şu anda işletim hub'ına bir başvuru almak (`_hubContext`) kullanarak `GlobalHost`.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir. Önceki bölümde tarayıcınızda görünür bir farkı olmayacak, ancak istemciye gönderilen ileti sayısını kısıtlanır.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>İstemcide düzgün animasyon ekleme

Uygulama neredeyse tamamlandı, ancak biz yanıt olarak sunucu ileti taşınırken bir daha fazla geliştirme istemcide şeklin halindeki yapabilir. Sunucu tarafından verilen yeni bir konuma şekil konumunu ayarlamak yerine JQuery kullanıcı Arabirimi kitaplığın kullanacağız `animate` şekli sorunsuz geçerli ve yeni konumu arasında taşımak için işlevi.

1. İstemci güncelleştirme `updateShape` yöntemi aramak için aşağıdaki vurgulanmış kodu ister:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Yukarıdaki kod şekli eski konumundan (Bu durumda, 100 milisaniye) animasyon aralığı boyunca sunucu tarafından verilen yeni bir tane taşır. Yeni animasyon başlatılmadan önce şekli üzerinde çalışan herhangi bir önceki animasyon temizlenir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde taşımanız gerekir. Diğer pencere şeklinde hareketini hareketini gelen ileti başına bir kez ayarlanan yerine zaman içinde ilişkilendirilmiş daha az düzensiz görüntülenmesi gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Başka bir adım

Bu öğreticide, istemciler ve sunucular arasında yüksek frekanslı iletiler gönderen bir SignalR uygulama programı öğrendiniz. Bu iletişim paradigma çevrimiçi oyunlar ve diğer simülasyonları gibi geliştirmek için kullanışlıdır [SignalR ile oluşturulan ShootR game](http://shootr.signalr.net).

Bu öğreticide oluşturduğunuz uygulamanın indirilebileceğini [kod Galerisi](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

SignalR geliştirme kavramları hakkında daha fazla bilgi edinmek için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
