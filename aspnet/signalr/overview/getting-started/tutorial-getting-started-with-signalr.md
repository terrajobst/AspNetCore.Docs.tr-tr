---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR 2 ile gerçek zamanlı bir sohbet | Microsoft Docs'
author: pfletcher
description: Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. SignalR için boş bir ASP.NET web uygulamasına ekleyin.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098630"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Öğretici: SignalR 2 ile gerçek zamanlı bir sohbet

Bu öğreticide SignalR gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Boş bir ASP.NET web uygulaması için SignalR eklediğinizde ve gönderin ve iletileri görüntülemek için bir HTML sayfası oluşturun.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Projesi kurun
> * Örneği çalıştırma
> * Kod İnceleme

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.

## <a name="set-up-the-project"></a>Projesi kurun

Bu bölümde Visual Studio 2017 ve SignalR 2 boş bir ASP.NET web uygulaması oluşturmak için nasıl kullanılacağını gösterir, SignalR ekleyin ve sohbet uygulaması oluşturma.

1. Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.

    ![Web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)

1. İçinde **yeni ASP.NET projesi - SignalRChat** penceresinde bırakın **boş** seçin ve seçilen **Tamam**.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - SignalRChat**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.

1. Sınıf adı *ChatHub* ve projeye ekleyin.

    Bu adımda oluşturulur *ChatHub.cs* sınıf dosyası ve bir dizi komut dosyaları ve projeye Signalr'yi destekleyen derleme başvurularını ekler.

1. Yeni bir kodu *ChatHub.cs* bu kodla sınıf dosyası:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - SignalRChat** seçin **yüklü** > **Visual C#**   >  **Web** ve ardından seçin **OWIN başlangıç sınıfı**.

1. Sınıf adı *başlangıç* ve projeye ekleyin.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **HTML sayfası**.

1. Yeni sayfa adı *dizin* seçip **Tamam**.

1. İçinde **Çözüm Gezgini**, oluşturduğunuz HTML sayfasının sağ tıklayıp **Başlangıç Sayfası Ayarla**.

1. HTML sayfasındaki varsayılan kodu şu kodla değiştirin:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. İçinde **Çözüm Gezgini**, genişletme **betikleri**.

    JQuery ve SignalR için betik kitaplıkları projesinde görünür.

    > [!IMPORTANT]
    > Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yüklemiş olabilirsiniz.

1. Kod bloğunun içinde komut dosyası başvuruları projeye komut dosyalarında sürümlerine karşılık geldiğinden emin olun.

    Özgün kod bloğunun komut dosyası başvuruları:
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Bunlar eşleşmiyorsa, güncelleştirme *.html* dosya.

1. Menü çubuğundan seçin **dosya** > **Tümünü Kaydet**.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Araç çubuğunda, açma **betik hata ayıklamasını** ve hata ayıklama modunda örneği çalıştırmak için Yürüt düğmesini seçin.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image3.png)

1. Tarayıcı açıldığında, sohbet kimliğinizi için bir ad girin.

1. Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.

1. Her tarayıcıda benzersiz bir ad girin.

1. Şimdi bir yorum girip seçin ekleyin **Gönder**. Bu, diğer tarayıcılarda yineleyin. Açıklamalar, gerçek zamanlı olarak görünür.

    > [!NOTE]
    > Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcılar yorum yayınlar. Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.

    Sohbet uygulaması üç farklı tarayıcılarda nasıl çalıştığını görürsünüz. Tüm tarayıcılar, Tom, Anand ve Susan iletileri gönderirken, gerçek zamanlı olarak güncelleştirin:

    ![Üç tüm tarayıcılar aynı Sohbet geçmişini görüntüleme](tutorial-getting-started-with-signalr/_static/image4.png)

1. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü. Adlı bir betik dosyası *hubs* , çalışma zamanında SignalR kitaplığı oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.

    ![Betik dosyaları düğümüne otomatik olarak oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Kod İnceleme

SignalRChat uygulaması, iki temel SignalR geliştirme görevlerini gösterir. Bir hub'ı oluşturma işlemini gösterir. Sunucu ana koordinasyon nesnesi olarak, hub'ı kullanır. Hub SignalR jQuery kitaplığı ileti göndermek ve almak için kullanır.

### <a name="signalr-hubs-in-the-chathubcs"></a>SignalR hub'ları ChatHub.cs

Yukarıdaki kod örneğinde `ChatHub` sınıf türetilir `Microsoft.AspNet.SignalR.Hub` sınıfı. Öğesinden türetme `Hub` SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır. Genel yöntemler hub sınıfınıza oluşturabilir ve sonra bir web sayfasında komut dosyalarından çağırarak bu yöntemi kullanın.

İstemciler sohbet kodda çağrı `ChatHub.Send` yeni bir ileti göndermek için yöntemi. Hub'ın ardından iletiyi tüm istemcilere çağırarak gönderir `Clients.All.broadcastMessage`.

`Send` Yöntemi birkaç hub kavramları göstermektedir:

* İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.

* Kullanım `Microsoft.AspNet.SignalR.Hub.Clients` tüm istemcilerle iletişim kurmak için dinamik özellik bu hub'a bağlı.

* İstemcide bir işlevi çağırmayı (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR ve jQuery index.html içinde

*İndex.html* sayfası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir. Kod, birçok önemli görevleri gerçekleştirir. Bu hub başvurmak için bir proxy bildirir, sunucunun anında içeriğin istemciler için çağrı yapabilirsiniz ve hub'ına ileti göndermek için bir bağlantı başlatır bir işlev bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> JavaScript'te, sunucu sınıfını ve üyelerini başvuru camelCase olması gerekir. Kod örneği başvuru C# *ChatHub* JavaScript olarak sınıfında `chatHub`.

Bu kod bloğu içinde bir geri çağırma işlevini betiğinde oluşturun.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. İki satırları, bu HTML içeriğini görüntülemeden önce isteğe bağlıdır ve kod eklemesini engellemek için en iyi yolu Göster kodlayın.

Bu kod hub'ı ile bir bağlantı açar.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce kodu bir bağlantı kurar sağlar.

Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasındaki düğmesi.

## <a name="additional-resources"></a>Ek kaynaklar

SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [SignalR projesi](http://signalr.net)

* [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide:

> [!div class="checklist"]
> * Projesi kurun
> * Örnek çalıştı
> * Dio

SignalR ve MVC 5 nasıl kullanılacağını öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [SignalR 2 ve MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)