---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Öğretici: SignalR 2 ve MVC 5 kullanmaya başlama | Microsoft Docs'
author: pfletcher
description: Bu öğreticide, ASP.NET SignalR 2 gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulaması için SignalR ekleyin ve bir sohbet görünümü oluştur...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 4a4c013ff047f18f9d9b88595af7951577f3f200
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838656"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Öğretici: SignalR 2 ve MVC 5 kullanmaya başlama
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Projeyi yükle](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Bu öğreticide, ASP.NET SignalR 2 gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. Bir MVC 5 uygulaması için SignalR ekleyin ve gönderin ve iletileri görüntülemek için bir sohbet görünüm oluşturun. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
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

Bu öğreticide, ASP.NET SignalR 2 ve ASP.NET MVC 5 ile gerçek zamanlı bir web uygulaması geliştirme tanıtır. Öğreticide aynı sohbet uygulama kodunu [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak bir MVC 5 uygulaması için ekleme işlemi gösterilmektedir.

Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:

- SignalR kitaplığı için bir MVC 5 uygulaması ekleniyor.
- Hub ve içeriği istemcilere göndermek için OWIN başlangıç sınıflar oluşturma.
- İleti göndermek ve hub'ından güncelleştirmeleri görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanıyor.

Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan tamamlanmış sohbet uygulaması gösterir.

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Bölümler:

- [Projesi kurun](#setup)
- [Örneği çalıştırma](#run)
- [Kod İnceleme](#code)
- [Sonraki adımlar](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Projesi kurun

Önkoşullar:

- Visual Studio 2013. Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.

Bu bölümde, bir ASP.NET MVC 5 uygulaması oluşturma, SignalR kitaplığa ekleyin ve sohbet uygulaması oluşturma işlemi gösterilmektedir.

1. Visual Studio'da .NET Framework 4.5 hedefleyen bir C# ASP.NET uygulaması oluşturma, SignalRChat adlandırın ve Tamam'a tıklayın.

    ![Web oluşturma](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. İçinde `New ASP.NET Project` iletişim ve select **MVC**, tıklatıp **kimlik doğrulamayı Değiştir**.

    ![Web oluşturma](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Seçin **kimlik doğrulaması yok** içinde **kimlik doğrulamayı Değiştir** iletişim seçeneğine tıklayıp **Tamam**.

    ![Kimlik doğrulaması Yok'u seçin](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Uygulamanız için farklı kimlik doğrulama sağlayıcısı seçerseniz bir `Startup.cs` sınıfı sizin için oluşturulacaktır; kendi oluşturmanız gerekmez `Startup.cs` 10 aşağıdaki adımda sınıfı.
4. Tıklayın **Tamam** içinde **yeni ASP.NET projesi** iletişim.
5. Açık **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın. Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvuruları projeye ekler.

    `install-package Microsoft.AspNet.SignalR`
6. İçinde **Çözüm Gezgini**, komut dosyaları klasörünü genişletin. SignalR için betik kitaplıkları projeye eklendiğini unutmayın.

    ![Betikleri klasörü](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | Yeni klasör**, adlı yeni bir klasör ekleyin **Hubs**.
8. Sağ **Hubs** klasörü tıklatın **Ekle | Yeni öğe**seçin **Visual C# | Web | SignalR** düğümünde **yüklü** bölmesinde **SignalR Hub sınıfı (v2)** Orta bölmedeki ve adlı yeni bir hub'ı oluşturma **ChatHub.cs**. Bu sınıf, tüm istemciler için iletileri gönderen bir SignalR sunucu hub olarak kullanır.

    ![Yeni hub'ı oluşturma](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Değiştirin **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Startup.cs adlı yeni bir sınıf oluşturun. Dosyanın içeriğini aşağıdaki gibi değiştirin.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Düzen `HomeController` sınıfı bulundu **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem döndürür **sohbet** daha sonraki bir adımda oluşturacağınız görünümü.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Sağ **görünümler/giriş** klasörü ve select **Ekle... | Görünüm**.
13. İçinde **Görünüm Ekle** iletişim kutusunda, yeni görünüm adı **sohbet**.

    ![Görünüm ekleme](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Öğesinin içeriğini değiştirin **Chat.cshtml** aşağıdaki kod ile.

    > [!IMPORTANT]
    > Visual Studio projenize SignalR ve diğer betik kitaplıkları eklemek, Paket Yöneticisi SignalR betik dosyasının bu konuda gösterilen sürümden daha yeni bir sürümü yükleyebilirsiniz. Kodunuzda betik başvurusu betik kitaplığını projenize yüklü sürümü eşleştiğinden emin olun.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
2. Tarayıcının adres satırındaki ekleme **/home/sohbet** proje için varsayılan sayfasının URL'si. Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri sohbet sayfayı yüklemez.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Bir kullanıcı adı girin.
4. Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın. Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.
5. Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**. Açıklamalar, hepsinin tarayıcı görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcılar yorum yayınlar. Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.
6. Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan sohbet uygulaması gösterir.

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü. Internet Explorer tarayıcı olarak kullanıyorsanız bu hata ayıklama modunda görünür bir düğümdür. Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir. Internet Explorer dışındaki bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hubs** atarak ona doğrudan, örneğin dosya http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Kod İnceleme

SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı. Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır. Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişin.

İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi. Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.addNewMessageToPage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için özelliği bu hub'a bağlı.
- İstemcide bir işlevi çağırmayı (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir. Önemli görevleri kod hub'ına ileti göndermek için sunucu istemcilere anında iletme içeriğe çağırabilen bir işlevi bildirmek ve bağlantı başlatma otomatik olarak oluşturulan proxy hub'ına yönelik bir başvuru oluşturuyor.

Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> JavaScript'te, sunucu sınıfını ve üyelerini ortası büyük harf başvurudur. Kod örneği C# başvuran **ChatHub** JavaScript olarak sınıfında **chatHub**. Başvuru istiyorsanız `ChatHub` sınıfı ile geleneksel Pascal jQuery içinde C# dilinde olduğu gibi büyük/küçük harfleri ChatHub.cs sınıf dosyasını düzenleyin. Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı. Ardından Ekle `HubName` özniteliğini `ChatHub` sınıfından, örneğin `[HubName("ChatHub")]`. Son olarak, jQuery başvuru güncelleştirme `ChatHub` sınıfı.


Aşağıdaki kod, komut dosyasında bir geri çağırma işlevini oluşturulacağını gösterir. Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. İsteğe bağlı çağrısı `htmlEncode` kod eklemesini engellemek için bir yol olarak sayfasını görüntülemeden önce ileti içeriği kodlama HTML şekilde işlev gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir. Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasında düğme.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz. Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.

Örnek SignalR uygulamayı Azure'a dağıtma hakkında kılavuz için bkz. [Azure App service'taki Web Apps ile SignalR kullanarak](../deployment/using-signalr-with-azure-web-sites.md). Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
