---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: "Yüksek yoğunlukta gerçek zamanlı SignalR ile 1.x | Microsoft Docs"
author: pfletcher
description: "Bu öğretici yüksek sıklıkta Mesajlaşma işlevleri sağlamak için ASP.NET SignalR kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Yüksek içinde ileti sıklıkta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/16/2013
ms.topic: article
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 0c680a7d8b911b2734647948b683d5ff6e47aec4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="high-frequency-realtime-with-signalr-1x"></a>Yüksek yoğunlukta gerçek zamanlı SignalR ile 1.x
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu öğretici yüksek sıklıkta Mesajlaşma işlevleri sağlamak için ASP.NET SignalR kullanan bir web uygulamasının nasıl oluşturulacağını gösterir. Yüksek yoğunlukta Mesajlaşma bu durumda sabit bir oranda gönderilen güncelleştirmeler anlamına gelir; Bu uygulama söz konusu olduğunda, 10'a kadar saniyenin iletileri.
> 
> Bu öğreticide oluşturacaksınız uygulama kullanıcıların sürükleyebilirsiniz bir şekli görüntüler. Şeklin konumunu diğer bağlı tarayıcılarda Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde güncelleştirilecektir.
> 
> Bu öğreticide sunulan kavramlar gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamalar vardır.
> 
> Öğretici yorumları Hoş Geldiniz. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com).


## <a name="overview"></a>Genel Bakış

Bu öğretici bir nesnenin durumunu gerçek zamanlı diğer tarayıcılarda paylaşan bir uygulamasının nasıl oluşturulacağını gösterir. Oluşturacağız uygulama MoveShape adı verilir. MoveShape sayfa kullanıcı sürükleyebilirsiniz bir HTML Div öğesinin görüntülenir; Kullanıcı Div sürüklendiğinde yeni konumunu ardından eşleştirilecek şeklin konumunu güncelleştirmek için diğer tüm bağlı istemcileri söyleyecektir sunucusuna gönderilir.

![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

Bu öğreticide oluşturduğunuz uygulama tarafından Damian Edirneli demo üzerinde temel alır. Bu Tanıtım içeren bir video görülebilir [burada](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR).

Öğretici şekli sürüklenen gibi tetiklenen her olay SignalR iletileri göndermek nasıl gösteren tarafından başlatılır. Her bağlı istemci sonra her zaman bir ileti alındığında şekli yerel sürümünü konumunu güncelleştirir.

Uygulama bu yöntemi kullanarak çalışır, ancak olurdu bu yana, böylece istemciler ve sunucu iletileri ile çok sayıda ve performansının düşmesine neden gönderilen iletisi sayısı için üst sınır bu önerilen bir programlama modeli değil . İstemci üzerinde görüntülenen animasyon de kopuk şekli hemen her yöntemi tarafından taşındı yerine her yeni konuma sorunsuz taşıma olacaktır. Öğreticinin sonraki bölümlerde iletileri istemci veya sunucu tarafından gönderilme en yüksek hızı kısıtlayan bir zamanlayıcı işlevi oluşturma ve Şekil sorunsuz konumlar arasında taşıma gösterilmektedir. Bu öğreticide oluşturduğunuz uygulamanın son sürümü karşıdan yüklenebilir [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

Bu öğretici aşağıdaki bölümleri içerir:

- [Önkoşulları](#prerequisites)
- [Proje oluşturma](#createtheproject)
- [ASP.NET SignalR ve JQuery.UI NuGet paketleri ekleme](#nugetpackages)
- [Temel uygulama oluşturma](#baseapp)
- [İstemci döngü ekleme](#clientloop)
- [Sunucu döngü ekleme](#serverloop)
- [İstemcide düzgün animasyon ekleme](#animation)
- [Başka bir adım](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide Visual Studio 2012 veya Visual Studio 2010 gerektirir. Visual Studio 2010 kullanılırsa, proje .NET Framework 4.5 yerine .NET Framework 4 kullanır.

Visual Studio 2012 kullanıyorsanız, yüklediğiniz önerilir [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650). Bu güncelleştirme yayımlama, yeni işlevlere geliştirmeler gibi yeni özellikler içerir ve yeni şablonlar.

Visual Studio 2010 varsa, emin olun [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklenir.

<a id="createtheproject"></a>

## <a name="create-the-project"></a>Projeyi oluşturma

Bu bölümde, Visual Studio Proje oluşturacağız.

1. Gelen **dosya** menüsünü tıklatın **yeni proje**.
2. İçinde **yeni proje** iletişim kutusunda, genişletin **C#** altında **şablonları** seçip **Web**.
3. Seçin **ASP.NET boş Web uygulaması** şablonu, proje adı *MoveShapeDemo*, tıklatıp **Tamam**.

    ![Yeni proje oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>SignalR ve JQuery.UI NuGet paketleri ekleme

NuGet paketini yükleyerek bir projeye SignalR işlevselliği ekleyebilirsiniz. Sürüklenen ve animasyonlu şekle izin vermek için bu öğreticiyi de JQuery.UI paketini kullanın.

1. Tıklatın **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**.
2. Paket Yöneticisi'nde aşağıdaki komutu girin.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR paketi diğer NuGet paket sayısı bağımlılıklar olarak yükler. Yükleme tamamlandığında, bir ASP.NET uygulamasındaki SignalR kullanmak için gereken tüm sunucu ve istemci bileşenleri vardır.
3. Paket Yöneticisi konsolunda JQuery ve JQuery.UI paketleri yüklemek için aşağıdaki komutu girin.

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>Temel uygulama oluşturma

Bu bölümde, Şekil konumunu sunucuya her fare hareketi olayını sırasında gönderir. bir tarayıcı uygulaması oluşturacağız. Alındığında sunucu daha sonra bu bilgileri diğer tüm bağlı istemcileri için yayınlar. Sonraki bölümlerde bu uygulamada üzerinde genişletin.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle**, **sınıfı...** . Sınıf adını **MoveShapeHub** tıklatıp **Ekle**.
2. Kod yeni değiştirin **MoveShapeHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub` Sınıfı yukarıdaki bir SignalR hub'ının bir uygulamasıdır. Olarak [SignalR ile çalışmaya başlama](index.md) öğretici, hub istemcileri doğrudan çağıran bir yöntem vardır. Bu durumda, istemci içeren yeni bir nesne gönderir ardından diğer tüm bağlı istemcilere yayımladınız sunucusuna şeklin X ve Y koordinatları. SignalR otomatik olarak JSON kullanarak bu nesneyi seri hale.

    İstemciye gönderilecek nesne (`ShapeModel`) şeklin konumunu depolamak için üye içeriyor. Belirtilen bir istemci, kendi veri gönderilmez böylece sunucu üzerindeki nesnesi sürümünü hangi istemci verilerinin depolanıyorsa, izlemek için üye de içerir. Bu üyeyi kullanan `JsonIgnore` serileştirilmiş ve istemciye gönderilen tutmak için öznitelik.
3. Ardından, uygulama başlatıldığında hub yaparız. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Genel uygulama sınıfı**. Varsayılan adı kabul *Global* tıklatıp **Tamam**.

    ![Genel uygulama sınıfı ekleme](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. Aşağıdakileri ekleyin `using` sağlanan sonra deyimi **kullanarak** Global.asax.cs sınıfı deyimlerinde.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. Aşağıdaki kod satırını ekleyin `Application_Start` SignalR için varsayılan yolu kaydetmek için genel sınıfının yöntemi.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Global.asax dosyası aşağıdaki gibi görünmelidir:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. Ardından, istemci ekleyeceğiz. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **Html sayfası**. Sayfa uygun bir ad verin (gibi **Default.html**) tıklatıp **Ekle**.
7. İçinde **Çözüm Gezgini**, az önce oluşturduğunuz sayfanın sağ tıklatın ve **Başlangıç Sayfası Ayarla**.
8. HTML sayfası varsayılan kodda aşağıdaki kod parçacığıyla değiştirin.

    > [!NOTE]
    > Betik betikler klasörüne projenizde eklenen paketler eşleşme başvurduğundan emin olun. Visual Studio 2010'da, JQuery ve projeye eklenen SignalR sürümüyle sürüm numaralarını eşleşmiyor.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    Yukarıdaki HTML ve JavaScript kodu şekli adlı bir kırmızı Div oluşturur, jQuery kitaplığını kullanarak şeklin sürükleme davranışı etkinleştirir ve şeklin kullanır `drag` şeklin konumu sunucusuna göndermek için olay.
9. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>İstemci döngü ekleme

Şekil her fare hareketi olayını üzerindeki konumunu gönderme gerekli olmayan miktarda ağ trafiği oluşturacak olduğundan, istemciden gelen iletileri kısıtlanan gerekir. Javascript kullanacağız `setInterval` sabit bir oranda sunucuya yeni konum bilgilerini gönderir bir döngü ayarlamak için işlevi. Bu döngü, bir "Oyun döngü", oyun veya diğer benzetimi işlevselliğini tüm sürücüleri art arda çağrılan bir işlev çok basit bir gösterimidir.

1. Aşağıdaki kod parçacığını eşleşecek şekilde HTML sayfasındaki istemci kodunu güncelleştirin.

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    Yukarıdaki güncelleştirme ekler `updateServerModel` sabit frekansında çağrılır işlevi. Bu işlev konum verileri sunucuya gönderdiği her `moved` bayrağı göndermek için yeni konum verileri olduğunu gösterir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir. Animasyon, sunucuya gönderilen ileti sayısını kısıtlanacak olduğundan, olduğu gibi önceki bölümde kesintisiz olarak görünmez.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>Sunucu döngü ekleme

Geçerli uygulamada sunucudan istemciye gönderilen iletileri alınana kadar sık gider. İstemcide görülen benzer bir sorun gösterir; iletiler daha sık gerekli değildir ve bağlantı sonucunda taşan hale gelebilir gönderilebilir. Bu bölümde, giden iletiler oranını kısıtlar bir süreölçer uygulamak için sunucuyu güncelleştirmek açıklar.

1. Değiştir `MoveShapeHub.cs` aşağıdaki kod parçacığını ile.

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    Yukarıdaki kod eklemek için istemci genişletir `Broadcaster` giden kısıtlar sınıfı iletileri kullanarak `Timer` .NET framework sınıf.

    Hub geçici olduğundan (gerektiğinde her zaman oluşturulduğu), `Broadcaster` bir singleton olarak oluşturulur. Geç Başlatma (.NET 4'te tanıtılan) oluşturma, Zamanlayıcı başlamadan önce ilk hub örneği tamamen oluşturulduğundan emin olduktan gerektiğinde kadar erteleme kullanılır.

    İstemcilerin çağrısı `UpdateShape` işlevi sonra merkezin dışında taşınmış `UpdateModel` gelen iletileri hemen alınan her onun artık adlı şekilde yöntemi. Bunun yerine, iletileri istemcilere saniye başına 25 çağrı hızında gönderilir tarafından yönetilen `_broadcastLoop` Zamanlayıcı içinden `Broadcaster` sınıfı.

    Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıf gereken şu anda işletim hub için bir başvuru almak (`_hubContext`) kullanarak `GlobalHost`.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir. Önceki bölümde tarayıcıdan görünür bir fark olmayacak, ancak istemciye gönderilen ileti sayısını kısıtlanacak.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>İstemcide düzgün animasyon ekleme

Uygulama neredeyse tamamlandı, ancak yanıt olarak sunucu ileti taşınırken bir daha fazla geliştirme, istemcide şeklin hareketli vermiyoruz. Sunucu tarafından verilen yeni bir konuma şeklin konumunu ayarlamak yerine JQuery UI kitaplığın kullanacağız `animate` şekli sorunsuz geçerli ve yeni konumunu arasında taşımak için işlevi.

1. İstemcinin güncelleştirme `updateShape` aramak için yöntemi aşağıdaki vurgulanmış kodu ister:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    Yukarıdaki kod şekli eski konumdan animasyon aralığı (Bu durumda, 100 milisaniyede) seyri içinde sunucu tarafından verilen yeni bir taşır. Yeni animasyon başlamadan önce Şekil üzerinde çalışan herhangi bir önceki animasyon temizlenir.
2. F5 tuşuna basarak uygulamayı başlatın. Sayfanın URL'sini kopyalayın ve ikinci bir tarayıcı penceresine yapıştırın. Şeklin tarayıcı pencerelerini sürükleyin; bir tarayıcı penceresi şeklinde taşımanız gerekir. Diğer pencere şeklinde hareketini hareketini üzerinden gelen ileti başına bir kez ayarlanan yerine zaman ilişkilendirilir daha az düzensiz görüntülenmesi gerekir.

    ![Uygulama penceresi](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>Başka bir adım

Bu öğreticide, istemciler ve sunucular arasında yüksek sıklıkta iletileri gönderen bir SignalR uygulama programı nasıl öğrendiniz. Bu iletişim çevrimiçi oyunlar ve diğer benzetimleri gibi geliştirmek için yararlı bir örnektir [SignalR ile oluşturulan ShootR oyun](http://shootr.signalr.net).

Bu öğreticide oluşturduğunuz tam uygulama yüklenebilir [kod Galerisi'nden](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6).

SignalR geliştirme kavramları hakkında daha fazla bilgi edinmek için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
