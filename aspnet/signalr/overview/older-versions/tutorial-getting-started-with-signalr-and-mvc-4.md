---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4 | Microsoft Docs'
author: pfletcher
description: ASP.NET SignalR ve ASP.NET MVC 4 gerçek zamanlı sohbet uygulaması oluşturmak için kullanın.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873725"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Bu öğreticide, ASP.NET SignalR gerçek zamanlı sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. MVC 4 uygulama SignalR eklemek ve göndermek ve iletileri görüntülemek için bir sohbet görünüm oluşturun.


## <a name="overview"></a>Genel Bakış

Bu öğreticide, ASP.NET SignalR ve ASP.NET MVC 4 ile gerçek zamanlı web uygulaması geliştirme tanıtır. Öğretici aynı sohbet uygulama kodunu kullanır [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak Internet şablona dayalı bir MVC 4 uygulama eklemek gösterilmektedir.

Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:

- SignalR kitaplığı MVC 4 uygulamaya ekleme.
- İçeriği istemcilere göndermek için bir hub sınıfı oluşturma.
- İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.

Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan tamamlanmış sohbet uygulamayı gösterir.

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Bölümler:

- [Projesi ayarlayın](#setup)
- [Örnek çalıştırın](#run)
- [Kodu inceleyin](#code)
- [Sonraki adımlar](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Projesi ayarlayın

Önkoşullar:

- Visual Studio 2010 SP1, Visual Studio 2012 veya Visual Studio 2012 Express. Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.
- Visual Studio 2010 için yükleme [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Bu bölümde, bir ASP.NET MVC 4 uygulama oluşturmak, SignalR Kitaplığı eklemek ve sohbet uygulaması oluşturma gösterilmektedir.

1. 1. Visual Studio'da ASP.NET MVC 4 uygulama oluşturma, SignalRChat adlandırın ve Tamam'ı tıklatın.

        > [!NOTE]
        > VS 2010'da seçin **.NET Framework 4** Framework sürümü açılır denetimindeki. SignalR kodu .NET Framework sürüm 4 ve 4.5 çalışır.

        ![MVC web oluşturma](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Internet uygulaması şablonunu seçin, seçeneğini temizleyin **birim testi projesi oluşturma**, Tamam'ı tıklatın.

         ![MVC internet sitesi oluştur](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Açık **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın. Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvurularını projeye ekler.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. İçinde **Çözüm Gezgini** komut dosyaları klasörünü genişletin. SignalR için komut dosyası kitaplıkları projeye eklendiğini unutmayın.

         ![Kitaplık Başvurusu](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | Yeni klasör**, ve adlı yeni bir klasör ekleyin **hub**.
      6. Sağ **hub** klasörünü tıklatın **Ekle | Sınıf**ve adlı yeni bir C# sınıfı oluşturmanız **ChatHub.cs**. Bu sınıf, tüm istemcilere iletileri gönderen bir SignalR sunucu hub olarak kullanır.

> [!NOTE]
> Visual Studio 2012 kullanın ve yüklediyseniz [ASP.NET ve Web Araçları 2012.2 güncelleştirme](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), hub sınıfı oluşturmak için yeni SignalR öğe şablonu kullanabilirsiniz. Bunu yapmak için sağ **hub** klasörünü tıklatın **Ekle | Yeni öğe**seçin **SignalR hub'ı sınıfı (v1)** ve sınıf adını **ChatHub.cs**.


1. Kodla **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Açık **Global.asax** proje için dosya ve yöntemine bir çağrı ekleyin `RouteTable.Routes.MapHubs();` kod ilk satırı olarak `Application_Start` yöntemi. Bu kod, SignalR hub'ları için varsayılan rota kaydeder ve diğer yollar kaydetmeden önce çağrılmalıdır. Tamamlanan `Application_Start` yöntemi aşağıdaki gibi görünür.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Düzen `HomeController` sınıfı bulunan **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem **sohbet** bir sonraki adımda oluşturacağınız görünümü.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. İçinde sağ `Chat` yalnızca oluşturulur ve tıklatın yöntemi **Görünüm Ekle** yeni bir görünüm dosyası oluşturmak için.
5. İçinde **Görünüm Ekle** iletişim kutusunda, emin olun onay kutusunun seçili için **düzen veya ana sayfa kullanmak** (diğer onay kutularını temizleyin) ve ardından **Ekle**.

    ![Bir görünüm ekleyin](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Adlı yeni görünüm dosya düzenleme **Chat.cshtml**. Sonra &lt;h2&gt; etiketi, aşağıdaki yapıştırın &lt;div&gt; bölüm ve `@section scripts` sayfasına kod bloğu. Bu betiği, Sohbet iletilerini göndermek ve sunucu alınan iletileri görüntülemek için sayfanın sağlar. Sohbet görünüm için tam kod aşağıdaki kod bloğunda görüntülenir.

    > [!IMPORTANT]
    > Visual Studio projenizi SignalR ve diğer betik kitaplıkları eklediğinizde, Paket Yöneticisi bu konuda gösterilen sürümlerinden daha yeni olan komutlar sürümlerini yükleyebilirsiniz. Komut dosyası başvuruları kodunuzda projenizde yüklü betik kitaplıkları sürümleri eşleştiğinden emin olun.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örnek çalıştırın

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
2. Tarayıcının adres satırındaki append **/home/sohbet** proje için varsayılan sayfasının URL'si için. Bir tarayıcı örneği ve bir kullanıcı adı için ister sohbet sayfa yükler.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Bir kullanıcı adı girin.
4. Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın. Her tarayıcı örnek benzersiz bir kullanıcı adı girin.
5. Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**. Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcı yorumları yayınlar. Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.
6. Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir.

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm. Tarayıcı olarak Internet Explorer kullanıyorsanız, bu hata ayıklama modunda görünür bir düğümdür. Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir. Internet Explorer dışında bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hub** kendisine doğrudan, örneğin giderek dosya http://mywebsite/signalr/hubs.

    ![Oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Kodu inceleyin

SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı. Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır. Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında jQuery komut dosyalarından çağırarak erişim.

İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem. Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.addNewMessageToPage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için özelliğini bu hub'ına bağlı.
- İstemcide bir jQuery işlevi çağırma (aşağıdaki gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir. Kodda önemli görevleri hub'ına iletileri göndermek için sunucunun istemcilere anında içeriği için çağırabilirsiniz işlevi bildirme ve bağlantı başlatma otomatik olarak oluşturulan Proxy hub'ına yönelik bir başvuru oluşturuyorsunuz.

Aşağıdaki kod, bir hub için bir proxy bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> JQuery içinde sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur. Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**. Başvuru istiyorsanız `ChatHub` sınıfı jQuery geleneksel Pascal ile C# ' ta yaptığınız gibi büyük/küçük harfleri ChatHub.cs sınıfı dosyasını düzenleyin. Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı. Ardından ekleyin `HubName` özniteliğini `ChatHub` sınıfı, örneğin `[HubName("ChatHub")]`. Son olarak, jQuery referansı güncelleştirme `ChatHub` sınıfı.


Aşağıdaki kod komut dosyasındaki bir geri çağırma işlevini oluşturulacağını gösterir. Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. İsteğe bağlı arama `htmlEncode` kod eklemesini önlemeye yönelik bir yolu olarak sayfasını görüntülemeden önce ileti içeriği kodlamak HTML şekilde işlev gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir. Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasının düğmesini.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz. Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.

Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR Project](http://signalr.net)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
