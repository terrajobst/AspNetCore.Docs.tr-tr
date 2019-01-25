---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Öğretici: SignalR 2 ve MVC 5 ile gerçek zamanlı bir sohbet | Microsoft Docs'
author: bradygaster
description: Bu öğreticide, ASP.NET SignalR 2 gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulaması için SignalR eklersiniz.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1b02aecc68a93dbd6373ca5304530e76c9d0b6b5
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837007"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Öğretici: SignalR 2 ve MVC 5 ile gerçek zamanlı sohbet

Bu öğreticide, ASP.NET SignalR 2 gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulaması için SignalR eklediğinizde ve gönderin ve iletileri görüntülemek için bir sohbet görünüm oluşturun.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Projesi kurun
> * Örneği çalıştırma
> * Kod İnceleme

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.

## <a name="set-up-the-project"></a>Projesi kurun

Bu bölümde, Visual Studio 2017 ve SignalR 2 boş bir ASP.NET MVC 5 uygulaması oluşturma, SignalR kitaplığa ekleyin ve sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir.

1. Visual Studio'da .NET Framework 4.5 hedefleyen bir C# ASP.NET uygulaması oluşturma, SignalRChat adlandırın ve Tamam'a tıklayın.

    ![Web oluşturma](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. İçinde **yeni ASP.NET Web uygulaması - SignalRMvcChat**seçin **MVC** seçip **kimlik doğrulamayı Değiştir**.

1. İçinde **kimlik doğrulamayı Değiştir**seçin **kimlik doğrulaması yok** tıklatıp **Tamam**.

    ![Kimlik doğrulaması Yok'u seçin](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. İçinde **yeni ASP.NET Web uygulaması - SignalRMvcChat**seçin **Tamam**.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.

1. İçinde **Yeni Öğe Ekle - SignalRChat**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.

1. Sınıf adı *ChatHub* ve projeye ekleyin.

    Bu adımda oluşturulur *ChatHub.cs* sınıf dosyası ve bir dizi komut dosyaları ve projeye Signalr'yi destekleyen derleme başvurularını ekler.

1. Yeni bir kodu *ChatHub.cs* bu kodla sınıf dosyası:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **sınıfı**.

1. Yeni bir sınıf adı *başlangıç* ve projeye ekleyin.

1. Değiştirin *Startup.cs* bu kodla sınıf dosyası:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. İçinde **Çözüm Gezgini**seçin **denetleyicileri** > **HomeController.cs**.

1. Bu yönteme ekleme *HomeController.cs*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Bu yöntem döndürür **sohbet** sonraki adımlardan birinde oluşturduğunuz görünümü.

1. İçinde **Çözüm Gezgini**, sağ **görünümleri** > **giriş**seçip **Ekle**  >    **Görünüm**.

1. İçinde **Görünüm Ekle**, yeni görünüm adı **sohbet** seçip **Ekle**.

1. Öğesinin içeriğini değiştirin **Chat.cshtml** Bu kod ile:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. İçinde **Çözüm Gezgini**, genişletme **betikleri**.

    JQuery ve SignalR için betik kitaplıkları projesinde görünür.

    > [!IMPORTANT]
    > Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yüklemiş olabilirsiniz.

1. Kod bloğunun içinde komut dosyası başvuruları projeye komut dosyalarında sürümlerine karşılık geldiğinden emin olun.

    Özgün kod bloğunun komut dosyası başvuruları:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Bunlar eşleşmiyorsa, güncelleştirme *.cshtml* dosya.

1. Menü çubuğundan seçin **dosya** > **Tümünü Kaydet**.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Araç çubuğunda, açma **betik hata ayıklamasını** ve hata ayıklama modunda örneği çalıştırmak için Yürüt düğmesini seçin.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Tarayıcı açıldığında, sohbet kimliğinizi için bir ad girin.

1. Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.

1. Her tarayıcıda benzersiz bir ad girin.

1. Şimdi bir yorum girip seçin ekleyin **Gönder**. Bu, diğer tarayıcılarda yineleyin. Açıklamalar, gerçek zamanlı olarak görünür.

    > [!NOTE]
    > Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcılar yorum yayınlar. Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.

    Sohbet uygulaması üç farklı tarayıcılarda nasıl çalıştığını görürsünüz. Tüm tarayıcılar, Tom, Anand ve Susan iletileri gönderirken, gerçek zamanlı olarak güncelleştirin:

    ![Üç tüm tarayıcılar aynı Sohbet geçmişini görüntüleme](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü. Adlı bir betik dosyası *hubs* , çalışma zamanında SignalR kitaplığı oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.

    ![Betik dosyaları düğümüne otomatik olarak oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Kod İnceleme

SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir. Bir hub'ı oluşturma işlemini gösterir. Sunucu ana koordinasyon nesnesi olarak, hub'ı kullanır. Hub SignalR jQuery kitaplığı ileti göndermek ve almak için kullanır.

### <a name="signalr-hubs-in-the-chathubcs"></a>SignalR hub'ları ChatHub.cs

Kod örneğinde, `ChatHub` sınıf türetilir `Microsoft.AspNet.SignalR.Hub` sınıfı. Öğesinden türetme `Hub` SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır. Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişin.

İstemciler sohbet kodda çağrı `ChatHub.Send` yeni bir ileti göndermek için yöntemi. Hub sırayla ileti tüm istemcilere çağırarak gönderen `Clients.All.addNewMessageToPage`.

`Send` Yöntemi birkaç hub kavramları göstermektedir:

* İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.

* Kullanım `Microsoft.AspNet.SignalR.Hub.Clients` tüm istemcilerle iletişim kurmak için dinamik özellik bu hub'a bağlı.

* İstemcide bir işlevi çağırmayı (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR ve jQuery Chat.cshtml

*Chat.cshtml* görünüm dosyası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir.  Kod, birçok önemli görevleri gerçekleştirir. Bu, otomatik olarak oluşturulan proxy hub için bir başvuru oluşturur, içeriği istemcilere göndermek için sunucu çağırabilir ve hub'ına ileti göndermek için bir bağlantı başlatır bir işlev bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript'te, sunucu sınıfını ve üyelerini camelCase başvurudur. Kod örneği başvuru C# `ChatHub` JavaScript olarak sınıfında `chatHub`.

Bu kod bloğu içinde bir geri çağırma işlevini betiğinde oluşturun.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. İsteğe bağlı çağrısı `htmlEncode` sayfasında görüntülemeden önce ileti içeriği kodlama HTML şekilde işlev gösterir. Bu kod eklemesini engellemek üzere bir yoludur.

Bu kod hub'ı ile bir bağlantı açar.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bir bağlantı oluşturmanızı sağlar.

Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasında düğme.

## <a name="get-the-code"></a>Kodu alma

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Ek kaynaklar

SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [SignalR projesi](http://signalr.net)

* [SignalR GitHub ve örnekleri](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Projesi kurun
> * Örnek çalıştı
> * Dio

Yüksek frekanslı Mesajlaşma işlevleri sağlamak için ASP.NET SignalR 2 kullanan bir web uygulamasının nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Yüksek frekanslı Mesajlaşma ile web uygulaması](tutorial-high-frequency-realtime-with-signalr.md)