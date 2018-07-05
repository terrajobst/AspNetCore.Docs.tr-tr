---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4 | Microsoft Docs'
author: pfletcher
description: Bir gerçek zamanlı bir sohbet uygulaması oluşturmak için ASP.NET SignalR ve ASP.NET MVC 4 kullanın.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: cced40f65700eff073aa4cff4560a137a50bbc5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399970"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Öğretici: SignalR ile çalışmaya başlama 1.x ve MVC 4
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Bu öğreticide, ASP.NET Signalr'yi gerçek zamanlı bir sohbet uygulaması oluşturmak için nasıl kullanılacağını gösterir. SignalR MVC 4 uygulama ekleme ve gönderin ve iletileri görüntülemek için bir sohbet görünümü oluşturmak.


## <a name="overview"></a>Genel Bakış

Bu öğreticide, ASP.NET SignalR ve ASP.NET MVC 4 ile gerçek zamanlı bir web uygulaması geliştirme tanıtır. Öğreticide aynı sohbet uygulama kodunu [SignalR Başlarken Öğreticisi](tutorial-getting-started-with-signalr.md), ancak Internet şablonu temel alan bir MVC 4 uygulamaya ekleme işlemi gösterilmektedir.

Bu konuda aşağıdaki SignalR geliştirme görevleri öğreneceksiniz:

- SignalR kitaplığı, MVC 4 uygulamaya ekleme.
- İçeriği istemcilere göndermek için bir hub sınıf oluşturuluyor.
- İleti göndermek ve hub'ından güncelleştirmeleri görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanıyor.

Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan tamamlanmış sohbet uygulaması gösterir.

![Sohbet örnekleri](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Bölümler:

- [Projesi kurun](#setup)
- [Örneği çalıştırma](#run)
- [Kod İnceleme](#code)
- [Sonraki adımlar](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Projesi kurun

Önkoşullar:

- Visual Studio 2010 SP1, Visual Studio 2012 veya Visual Studio 2012 Express. Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.
- Visual Studio 2010 için yükleme [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

Bu bölümde, bir ASP.NET MVC 4 uygulama oluşturmak, SignalR kitaplığa ekleyin ve sohbet uygulaması oluşturma işlemi gösterilmektedir.

1. 1. Visual Studio'da ASP.NET MVC 4 uygulama oluşturma, SignalRChat adlandırın ve Tamam'a tıklayın.

        > [!NOTE]
        > VS 2010'da seçin **.NET Framework 4** Framework sürüm dropdown denetimi. SignalR kod, .NET Framework sürüm 4 ve 4.5 üzerinde çalışır.

        ![MVC web oluşturma](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Internet uygulaması şablonu seçin, seçeneğini temizleyin **birim testi projesi oluşturma**, Tamam'ı tıklatın.

         ![MVC internet sitesi oluşturma](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Açık **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve aşağıdaki komutu çalıştırın. Bu adım, bir dizi komut dosyaları ve SignalR işlevselliğini etkinleştirmek derleme başvuruları projeye ekler.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. İçinde **Çözüm Gezgini** betikleri klasörünü genişletin. SignalR için betik kitaplıkları projeye eklendiğini unutmayın.

         ![Kitaplık başvuruları](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | Yeni klasör**, adlı yeni bir klasör ekleyin **Hubs**.
      6. Sağ **Hubs** klasörü tıklatın **Ekle | Sınıf**ve adlı yeni bir C# sınıfı oluşturma **ChatHub.cs**. Bu sınıf, tüm istemciler için iletileri gönderen bir SignalR sunucu hub olarak kullanır.

> [!NOTE]
> Visual Studio 2012 kullanın ve yüklüyse [ASP.NET ve Web Araçları 2012.2 güncelleştirme](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), hub sınıfı oluşturmak için yeni SignalR öğe şablonu kullanabilirsiniz. Bunu yapmak için sağ **Hubs** klasörü tıklatın **Ekle | Yeni öğe**seçin **SignalR Hub sınıfı (v1)** ve sınıf adını **ChatHub.cs**.


1. Değiştirin **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Açık **Global.asax** proje için dosya ve yöntemine bir çağrı ekleyin `RouteTable.Routes.MapHubs();` kod ilk satırı olarak `Application_Start` yöntemi. Bu kod, SignalR hub'ları için varsayılan rota kaydeder ve başka bir yolun kaydetmeden önce çağrılmalıdır. Tamamlanan `Application_Start` yöntemi aşağıdaki örnekteki gibi görünür.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Düzen `HomeController` sınıfı bulundu **Controllers/HomeController.cs** ve sınıfına aşağıdaki yöntemi ekleyin. Bu yöntem döndürür **sohbet** daha sonraki bir adımda oluşturacağınız görünümü.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. İçinde sağ `Chat` az önce oluşturduğunuz ve tıklayın yöntemi **Görünüm Ekle** yeni bir görünüm dosyası oluşturmak için.
5. İçinde **Görünüm Ekle** iletişim kutusunda emin olun onay kutusunun seçili için **düzeni veya ana sayfayı kullan** (diğer onay kutularını temizleyin) ve ardından **Ekle**.

    ![Görünüm ekleme](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Adlı yeni görünüm dosyası Düzenle **Chat.cshtml**. Sonra &lt;h2&gt; etiketinde, aşağıdakini yapıştırın &lt;div&gt; bölümü ve `@section scripts` sayfasına kod bloğu. Bu betik, sohbet iletileri gönderme ve sunucudaki iletileri görüntülemek sayfanın sağlar. Sohbet görünümü için tam kod, aşağıdaki kod bloğu içinde görünür.

    > [!IMPORTANT]
    > Visual Studio projenize SignalR ve diğer betik kitaplıkları eklemek, Paket Yöneticisi betikleriyle bu konuda gösterilen sürümlerden daha yeni sürümlerini yükleyebilirsiniz. Kodunuzda komut dosyası başvuruları projenize yüklü betik kitaplıkları sürümleri eşleştiğinden emin olun.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın.
2. Tarayıcının adres satırındaki ekleme **/home/sohbet** proje için varsayılan sayfasının URL'si. Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri sohbet sayfayı yüklemez.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Bir kullanıcı adı girin.
4. Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın. Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.
5. Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**. Açıklamalar, hepsinin tarayıcı görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcılar yorum yayınlar. Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.
6. Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan sohbet uygulaması gösterir.

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü. Internet Explorer tarayıcı olarak kullanıyorsanız bu hata ayıklama modunda görünür bir düğümdür. Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir. Internet Explorer dışındaki bir tarayıcı kullanıyorsanız, dinamik da erişebilirsiniz **hubs** atarak ona doğrudan, örneğin dosya http://mywebsite/signalr/hubs.

    ![Oluşturulan hub betiği](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Kod İnceleme

SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı. Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır. Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında jQuery betiklerden çağırarak erişin.

İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi. Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.addNewMessageToPage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için özelliği bu hub'a bağlı.
- İstemcide jQuery işlev çağırma (gibi `addNewMessageToPage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

**Chat.cshtml** görünüm dosyası kod örneğinde SignalR jQuery kitaplığının bir SignalR hub'ı ile iletişim kurmak için nasıl kullanılacağını gösterir. Önemli görevleri kod hub'ına ileti göndermek için sunucu istemcilere anında iletme içeriğe çağırabilen bir işlevi bildirmek ve bağlantı başlatma otomatik olarak oluşturulan proxy hub'ına yönelik bir başvuru oluşturuyor.

Aşağıdaki kod, bir hub için bir proxy bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> JQuery içinde sunucu sınıfını ve üyelerini ortası büyük harf başvurudur. Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**. Başvuru istiyorsanız `ChatHub` sınıfı ile geleneksel Pascal jQuery içinde C# dilinde olduğu gibi büyük/küçük harfleri ChatHub.cs sınıf dosyasını düzenleyin. Ekleme bir `using` başvurmak için deyimi `Microsoft.AspNet.SignalR.Hubs` ad alanı. Ardından Ekle `HubName` özniteliğini `ChatHub` sınıfından, örneğin `[HubName("ChatHub")]`. Son olarak, jQuery başvuru güncelleştirme `ChatHub` sınıfı.


Aşağıdaki kod, komut dosyasında bir geri çağırma işlevini oluşturulacağını gösterir. Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. İsteğe bağlı çağrısı `htmlEncode` kod eklemesini engellemek için bir yol olarak sayfasını görüntülemeden önce ileti içeriği kodlama HTML şekilde işlev gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir. Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** sohbet sayfasında düğme.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz. Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.

Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
