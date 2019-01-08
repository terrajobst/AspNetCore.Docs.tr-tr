---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Öğretici: SignalR 2 ile yüksek sıklıkta gerçek zamanlı uygulama oluşturma | Microsoft Docs'
author: pfletcher
description: Bu öğretici, ASP.NET SignalR, yüksek frekanslı Mesajlaşma işlevleri sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098596"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>Öğretici: SignalR 2 ile yüksek sıklıkta gerçek zamanlı uygulama oluşturma

Bu öğreticide, yüksek frekanslı Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulaması oluşturma işlemi gösterilmektedir. Bu durumda, "yüksek sıklıkta ileti" sunucu sabit bir fiyat karşılığında güncelleştirmeler gönderir anlamına gelir. Saniyede en fazla 10 iletileri gönderir.

Oluşturduğunuz uygulama, kullanıcıların sürükleyebilirsiniz bir şekil görüntüler. Sunucu, Zamanlanmış güncelleştirmeleri kullanarak sürüklenen şekli konumunu eşleşecek şekilde tüm bağlı tarayıcıların şekil konumunu güncelleştirir.

Bu öğreticide tanıtılan kavramları, gerçek zamanlı oyun uygulamaları ve diğer benzetimi uygulamaları vardır.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Projesi kurun
> * Temel uygulama oluşturma
> * Uygulama başlatıldığında hub'ına eşleme
> * İstemci Ekle
> * Uygulamayı çalıştırma
> * İstemci döngü Ekle
> * Sunucu döngü Ekle
> * Kesintisiz animasyon ekleme

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.

## <a name="set-up-the-project"></a>Projesi kurun

Bu bölümde, Visual Studio 2017'de bir proje oluşturun.

Bu bölümde, boş bir ASP.NET Web uygulaması oluşturma ve SignalR ve jQuery.UI kitaplıkları eklemek için Visual Studio 2017'yi kullanmayı gösterir.

1. Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.

    ![Web oluşturma](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. İçinde **yeni ASP.NET Web uygulaması - MoveShapeDemo** penceresinde bırakın **boş** seçin ve seçilen **Tamam**.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - MoveShapeDemo**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.

1. Sınıf adı *MoveShapeHub* ve projeye ekleyin.

    Bu adımda oluşturulur *MoveShapeHub.cs* sınıf dosyası. Aynı anda bir dizi komut dosyaları ve projeye Signalr'yi destekleyen derleme başvurularını ekler.

1. Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

1. İçinde **Paket Yöneticisi Konsolu**, şu komutu çalıştırın:

    ```console
    Install-Package jQuery.UI.Combined
    ```

    Komut, jQuery kullanıcı Arabirimi kitaplığı yükler. Şekil animasyon uygulamak için kullanın.

1. İçinde **Çözüm Gezgini**, betikleri düğümünü genişletin.

    ![Komut dosyası kitaplık başvuruları](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    Betik kitaplıkları için jQuery jQueryUI ve SignalR projesinde görünür.

## <a name="create-the-base-application"></a>Temel uygulama oluşturma

Bu bölümde, bir tarayıcı uygulaması oluşturun. Uygulama, her fare hareketi olayını sırasında şekil konumunu sunucusuna gönderir. Sunucu, bu bilgiler diğer bağlı istemcilere gerçek zamanlı olarak yayınlar. Sonraki bölümlerde bu uygulama hakkında daha fazla bilgi.

1. Açık *MoveShapeHub.cs* dosya.

1. Değiştirin *MoveShapeHub.cs* Bu kod dosyası:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. Dosyayı kaydedin.

`MoveShapeHub` Uygulaması bir SignalR hub'ın bir sınıftır. Olarak [SignalR ile çalışmaya başlama](tutorial-getting-started-with-signalr.md) Öğreticisi, hub'ına istemcileri doğrudan çağıran bir yöntem. Bu durumda, bir nesne yeni X ve Y koordinatları sunucuya şeklin istemci gönderir. Bu koordinatları, diğer tüm bağlı istemcileri yayımladınız. SignalR otomatik olarak JSON'ı kullanarak bu nesneyi serileştirir.

Uygulama gönderen `ShapeModel` istemciye nesne. Şeklin konum depolamak için üyeleri var. Sunucuda nesnenin hangi istemci verilerinin depolandığını izlemek için bir üyesi de sahiptir. Bu nesne, sunucunun geri kendisine bir istemcinin veri göndermesini engeller. Bu üyeyi kullanan `JsonIgnore` uygulama verileri seri hale getirme ve istemciye geri göndermeden tutmak için özniteliği.

## <a name="map-to-the-hub-when-app-starts"></a>Uygulama başlatıldığında hub'ına eşleme

Ardından, uygulama başlatıldığında hub'ına eşleme ayarlayın. SignalR 2'de bir OWIN başlangıç sınıfı ekleme eşleme oluşturur.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - MoveShapeDemo** seçin **yüklü** > **Visual C#**   >  **Web** ve ardından seçin **OWIN başlangıç sınıfı**.

1. Sınıf adı *başlangıç* seçip **Tamam**.

1. Varsayılan kodda değiştirin *Startup.cs* Bu kod dosyası:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

OWIN başlangıç sınıfı çağrıları `MapSignalR` zaman uygulama yürütür `Configuration` yöntemi. OWIN'ın başlangıç sınıfa işlemi kullanarak uygulamanın eklediği `OwinStartup` derleme özniteliği.

## <a name="add-the-client"></a>İstemci Ekle

HTML sayfası için istemci ekleyin.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **HTML sayfası**.

1. Sayfayı adlandırın **varsayılan** seçip **Tamam**.

1. İçinde **Çözüm Gezgini**, sağ *Default.html* seçip **Başlangıç Sayfası Ayarla**.

1. Varsayılan kodda değiştirin *Default.html* Bu kod dosyası:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. İçinde **Çözüm Gezgini**, genişletme **betikleri**.

    JQuery ve SignalR için betik kitaplıkları projesinde görünür.

    > [!IMPORTANT]
    > Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yükler.

1. Komut dosyalarını projedeki sürümleri değerine karşılık gelen kod bloğundaki komut dosyası başvuruları güncelleştirin.

Bu HTML ve JavaScript kodu kırmızı oluşturur `div` adlı `shape`. JQuery kitaplığını kullanarak şeklin sürükleyerek davranışını etkinleştirir ve kullandığı `drag` şeklin konum sunucuya göndermek için olay.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırdığınız için se'e çalışabilirsiniz. Bir tarayıcı penceresi şekli sürüklediğinizde, Şekil diğer tarayıcılarda çok taşır.

1. Araç çubuğunda, açma **betik hata ayıklamasını** ve ardından uygulamayı hata ayıklama modunda çalıştırmak için Yürüt düğmesini seçin.

    ![Hata ayıklama modu ve Çalıştır'ı seçerek kullanıcı görüntüsü.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    Sağ üst köşesindeki kırmızı şeklinde bir tarayıcı penceresi açılır.

1. Sayfanın URL'sini kopyalayın.

1. Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.

1. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde izler.

Uygulama çalışırken İşlevler, bu yöntemi kullanarak, önerilen bir programlama modeli değil. Gönderilen ileti sayısı üst sınırı yoktur. Sonuç olarak, istemciler ve sunucu iletileri ile dolmasını ve performansı düşürür. Ayrıca, uygulama istemcide kopuk bir animasyon görüntüler. Şekil anında her yöntemle taşıdığı düzensiz animasyon gerçekleşir. Şekil sorunsuz her yeni bir konuma taşınırsa, daha iyidir. Ardından, bu sorunları gidermek nasıl öğrenin.

## <a name="add-the-client-loop"></a>İstemci döngü Ekle

Her fare hareketi olayını şeklinizde konumunu gönderme gereksiz miktarda ağ trafiği oluşturur. Uygulama, istemciden gelen iletileri kısıtlama gerekir.

Javascript kullanan `setInterval` sabit bir hızda sunucuya yeni konum bilgileri gönderen bir döngü ayarlamak için işlevi. Bu döngü bir "Oyun döngüsü." temel gösterimidir Oyun işlevselliğini tüm sürücüleri tekrar tekrar çağrılan bir işlevdir.

1. İstemci kodu değiştirin *Default.html* Bu kod dosyası:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > Komut dosyası başvuruları yeniden değiştirmek zorunda. Bunlar, projeyi betiklerde sürümleri eşleşmelidir.

    Bu yeni bir kod ekler `updateServerModel` işlevi. Bir sabit sıklığına adlı. İşlev konum verileri sunucusuna gönderir. her `moved` bayrağı, yeni konum verileri göndermek için olduğunu gösterir.

1. Uygulamayı başlatmak için Yürüt düğmesini seçin.

1. Sayfanın URL'sini kopyalayın.

1. Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.

1. Şekil tarayıcı pencerelerini birinde sürükleyin. Şekil bir tarayıcı penceresinde izler.

Uygulama animasyon kesintisiz olarak görünmez sunucuya gönderilen ileti sayısını kısıtlar bu yana ilk başta vermedi.

## <a name="add-the-server-loop"></a>Sunucu döngü Ekle

Geçerli uygulamada, sunucudan istemciye gönderilen iletileri alındıklarında sıklıkta gönderilir. İstemcide görüyoruz gibi benzer bir sorun bu ağ trafiğini gösterir.

Uygulamayı, ihtiyaç duyulan daha sık ileti gönderebilir. Sonuç olarak bağlantı yayılmamış olabilir. Bu bölümde, giden iletiler oranını kısıtlar Zamanlayıcı eklemek için sunucu güncelleştirme işlemi açıklanmaktadır.

1. Öğesinin içeriğini değiştirin `MoveShapeHub.cs` Bu kod ile:

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. Uygulamayı başlatmak için Yürüt düğmesini seçin.

1. Sayfanın URL'sini kopyalayın.

1. Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.

1. Şekil tarayıcı pencerelerini birinde sürükleyin.

Bu kod eklemek için istemci genişletir `Broadcaster` sınıfı. Yeni bir sınıf kullanarak giden iletiler kısıtlar `Timer` .NET Framework sınıfı.

Hub geçici olduğunu öğrenmek uygundur. Gereken her zaman oluşturulur. Uygulamayı oluşturur `Broadcaster` singleton olarak. Yavaş başlatma erteleneceği kullanan `Broadcaster`'s, gerekli olana kadar oluşturma. Bu, uygulamanın ilk hub örneği tamamen Zamanlayıcı başlatmadan önce oluşturduğu garanti eder.

İstemcilerin çağrısı `UpdateShape` işlevi ardından hub dışında taşınmış `UpdateModel` yöntemi. Artık her hemen uygulama gelen iletiler aldığında çağrılır. Bunun yerine, uygulama, saniye başına 25 çağrılarının bir hızda istemcilere iletileri gönderir. İşlem tarafından yönetilen `_broadcastLoop` içinden Zamanlayıcı `Broadcaster` sınıfı.

Son olarak, istemci yöntemi hub'dan doğrudan çağırmak yerine `Broadcaster` sınıfı şu anda çalışan bir başvuru almak için gereksinim duyduğu `_hubContext` hub. Başvuru ile alır `GlobalHost`.

## <a name="add-smooth-animation"></a>Kesintisiz animasyon ekleme

Neredeyse tamamlanmış bir uygulamadır, ancak biz tek daha fazla iyileştirme yapabilirsiniz. Uygulama sunucusu iletilere yanıt olarak istemcide şekli taşır. JQuery kullanıcı Arabirimi kitaplık sunucusu tarafından verilen yeni bir konuma şekil konumunu ayarlamak yerine kullanın `animate` işlevi. Bu şeklin sorunsuz geçerli ve yeni konumu arasında taşıyabilirsiniz.

1. İstemci güncelleştirme `updateShape` yönteminde *Default.html* dosyasının vurgulanan kod gibi:

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. Uygulamayı başlatmak için Yürüt düğmesini seçin.

1. Sayfanın URL'sini kopyalayın.

1. Başka bir tarayıcı açın ve adres çubuğuna URL'yi yapıştırın.

1. Şekil tarayıcı pencerelerini birinde sürükleyin.

Diğer pencere şeklinde hareketini daha az düzensiz görünür. Uygulama hareketi gelen ileti başına bir kez ayarlanan yerine zaman içinde ilişkilendirir.

Bu kod, yeni bir tane eski konumdan şekli taşır. Sunucu, animasyon zaman aralığı boyunca şekil konumunu sağlar. Bu durumda, 100 milisaniyeden kısadır. Uygulama, yeni animasyon başlatılmadan önce şekli üzerinde çalışan herhangi bir önceki animasyon temizler.

## <a name="additional-resources"></a>Ek kaynaklar

Yalnızca öğrendiğiniz hakkında iletişim paradigma çevrimiçi oyunlar ve diğer simülasyonları gibi geliştirmek için kullanışlıdır [SignalR ile oluşturulan ShootR game](https://shootr.azurewebsites.net/).

SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [SignalR projesi](http://signalr.net)

* [SignalR GitHub ve örnekleri](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Projesi kurun
> * Temel bir uygulama oluşturdu
> * Uygulama başlatıldığında hub'ına eşlendi
> * İstemci eklendi
> * Bir uygulamayı çalıştırdınız
> * İstemci döngüsü eklendi
> * Sunucu döngü eklendi
> * Eklenen kesintisiz animasyon

ASP.NET SignalR 2 sunucu yayın işlevselliği sağlamak için kullandığı bir web uygulamasının nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [SignalR 2 ile sunucu yayını](tutorial-server-broadcast-with-signalr.md)