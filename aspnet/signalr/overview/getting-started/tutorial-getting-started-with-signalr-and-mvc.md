---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "Öğretici: SignalR 2 ve MVC 5 ile çalışmaya başlama | Microsoft Docs"
author: pfletcher
description: "Bu öğretici ASP.NET SignalR 2 gerçek zamanlı sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulamaya SignalR ekleyebilir ve sohbet Görünüm Oluştur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Öğretici: SignalR 2 ve MVC 5 ile çalışmaya başlama
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Bu öğretici ASP.NET SignalR 2 gerçek zamanlı sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulamaya SignalR ekleyebilir ve göndermek ve iletileri görüntülemek için bir sohbet görünüm oluşturun. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

Bu öğreticide, ASP.NET SignalR 2 ve ASP.NET MVC 5 ile gerçek zamanlı web uygulaması geliştirme tanıtır. Öğretici aynı sohbet uygulama kodunu kullanır [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak bir MVC 5 uygulama eklemek gösterilmektedir.

Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:

- SignalR kitaplığı MVC 5 uygulamaya ekleme.
- Hub ve içeriği istemcilere göndermek için OWIN başlangıç sınıfları oluşturma.
- İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.

Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan tamamlanmış sohbet uygulamayı gösterir.

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Bölümler:

- [Projesi ayarlayın](#setup)
- [Örnek çalıştırın](#run)
- [Kodu inceleyin](#code)
- [Sonraki adımlar](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Projesi ayarlayın

Önkoşullar:

- Visual Studio 2013. Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.

Bu bölümde, bir ASP.NET MVC 5 uygulaması oluşturmak, SignalR Kitaplığı eklemek ve sohbet uygulaması oluşturma gösterilmektedir.

1. Visual Studio'da .NET Framework 4.5 hedefleyen bir C# ASP.NET uygulaması oluşturun, SignalRChat adlandırın ve Tamam'ı tıklatın.

    ![Web oluşturulamıyor](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. İçinde `New ASP.NET Project` iletişim ve select **MVC**, tıklatıp **kimlik doğrulamayı Değiştir**.

    ![Web oluşturulamıyor](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Seçin **doğrulaması yok** içinde **kimlik doğrulamayı Değiştir** iletişim ve tıklatın **Tamam**.

    ![Hiçbir kimlik doğrulama yöntemini seçin](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Uygulamanız için farklı kimlik doğrulama sağlayıcısı seçerseniz bir `Startup.cs` sınıfı sizin için oluşturulacaktır; kendi oluşturmanız gerekmez `Startup.cs` 10 aşağıdaki adımda sınıfı.
4. Tıklatın **Tamam** içinde **yeni ASP.NET projesi** iletişim.
5. Açık **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın. Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvurularını projeye ekler.

    `install-package Microsoft.AspNet.SignalR`
6. İçinde **Çözüm Gezgini**, komut dosyaları klasörünü genişletin. SignalR için komut dosyası kitaplıkları projeye eklendiğini unutmayın.

    ![Betik klasörü](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | Yeni klasör**, ve adlı yeni bir klasör ekleyin **hub**.
8. Sağ **hub** klasörünü tıklatın **Ekle | Yeni öğe**seçin **Visual C# | Web | SignalR** düğümünde **yüklü** bölmesinde, **SignalR hub'ı sınıfı (v2)** center bölmesinden ve adlı yeni bir hub'ı oluşturma **ChatHub.cs**. Bu sınıf, tüm istemcilere iletileri gönderen bir SignalR sunucu hub olarak kullanır.

    ![Yeni hub'ı Oluştur](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Kodla **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Haline adlı yeni bir sınıf oluşturun. Dosyanın içeriği aşağıdaki gibi değiştirin.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Düzen `HomeController` sınıfı bulunan **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem **sohbet** bir sonraki adımda oluşturacağınız görünümü.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Sağ **görünümler/giriş** klasörü ve select **Ekle... | Görünüm**.
13. İçinde **Görünüm Ekle** iletişim kutusunda, yeni görünüm adı **sohbet**.

    ![Bir görünüm ekleyin](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Değiştir **Chat.cshtml** aşağıdaki kod ile.

    > [!IMPORTANT]
    > Visual Studio projenizi SignalR ve diğer betik kitaplıkları eklediğinizde, Paket Yöneticisi SignalR komut dosyasının bu konuda gösterilen sürümden daha yeni bir sürümünü yükleyebilirsiniz. Komut başvurusu, kodunuzda projenizde yüklü kod kitaplığı sürümü eşleştiğinden emin olun.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örnek çalıştırın

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
2. Tarayıcının adres satırındaki append **/home/sohbet** proje için varsayılan sayfasının URL'si için. Bir tarayıcı örneği ve bir kullanıcı adı için ister sohbet sayfa yükler.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Bir kullanıcı adı girin.
4. Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın. Her tarayıcı örnek benzersiz bir kullanıcı adı girin.
5. Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**. Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcı yorumları yayınlar. Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.
6. Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir.

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm. Tarayıcı olarak Internet Explorer kullanıyorsanız, bu hata ayıklama modunda görünür bir düğümdür. Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir. Internet Explorer dışında bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hub** kendisine doğrudan, örneğin http://mywebsite/signalr/hubs giderek dosya.

<a id="code"></a>

## <a name="examine-the-code"></a>Kodu inceleyin

SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı. Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır. Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişim.

İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem. Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.addNewMessageToPage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için özelliğini bu hub'ına bağlı.
- İstemcide bir işlevi çağırmak (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir. Kodda önemli görevleri hub'ına iletileri göndermek için sunucunun istemcilere anında içeriği için çağırabilirsiniz işlevi bildirme ve bağlantı başlatma otomatik olarak oluşturulan Proxy hub'ına yönelik bir başvuru oluşturuyorsunuz.

Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript'te sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur. Kod örneği C# başvuran **ChatHub** JavaScript sınıfında **chatHub**. Başvuru istiyorsanız `ChatHub` sınıfı jQuery geleneksel Pascal ile C# ' ta yaptığınız gibi büyük/küçük harfleri ChatHub.cs sınıfı dosyasını düzenleyin. Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı. Ardından ekleyin `HubName` özniteliğini `ChatHub` sınıfı, örneğin `[HubName("ChatHub")]`. Son olarak, jQuery referansı güncelleştirme `ChatHub` sınıfı.


Aşağıdaki kod komut dosyasındaki bir geri çağırma işlevini oluşturulacağını gösterir. Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. İsteğe bağlı arama `htmlEncode` kod eklemesini önlemeye yönelik bir yolu olarak sayfasını görüntülemeden önce ileti içeriği kodlamak HTML şekilde işlev gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir. Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasının düğmesini.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz. Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.

Örnek SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md). Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR Project](http://signalr.net)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
