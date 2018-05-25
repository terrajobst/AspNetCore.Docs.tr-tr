---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x | Microsoft Docs'
author: pfletcher
description: Bir HTML sayfasında bir gerçek zamanlı sohbet uygulaması oluşturmak için ASP.NET SignalR kullanın.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Öğretici: SignalR ile çalışmaya başlama 1.x
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. Boş bir ASP.NET web uygulamasına SignalR ekleyebilir ve göndermek ve iletileri görüntülemek için bir HTML sayfası oluşturun.


## <a name="overview"></a>Genel Bakış

Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme sunmaktadır. Boş bir ASP.NET web uygulamasına SignalR Kitaplığı eklemek, istemcilere ileti göndermek için bir hub sınıf oluşturun ve sohbet ileti gönderme ve alma kullanıcıların olanak sağlayan bir HTML sayfası oluşturun. MVC görünümü kullanarak MVC 4'te bir sohbet uygulaması oluşturmak nasıl oluşturulduğunu gösteren benzer bir öğretici için bkz: [SignalR ve MVC 4 ile çalışmaya başlama](index.md).

> [!NOTE]
> Bu öğretici (1.x) sürümünü SignalR kullanır. SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için bkz: 1.x ve 2.0 [yükseltme SignalR 1.x projeleri](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR, Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmak için bir açık kaynak .NET kitaplığıdır. Sosyal uygulamalar, çok kullanıcılı oyunlar, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları örnek olarak verilebilir. Bunlar genellikle gerçek zamanlı uygulamaları olarak adlandırılır.

SignalR gerçek zamanlı uygulamaları oluşturma işlemini basitleştirir. Bir ASP.NET server kitaplığı ve istemci-sunucu bağlantıları yönetmek ve içerik güncelleştirmelerini istemcilere anında kolaylaştırmak için JavaScript istemci kitaplığını içerir. Gerçek zamanlı işlevselliği sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığı ekleyebilirsiniz.

Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:

- SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.
- İçeriği istemcilere göndermek için bir hub sınıfı oluşturma.
- İleti gönderme ve hub güncelleştirmelerini görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanma.

Aşağıdaki ekran görüntüsünde bir tarayıcıda çalışan sohbet uygulamayı gösterir. Her yeni kullanıcı yorumları gönderin ve kullanıcı sohbet katıldıktan sonra eklenen açıklamalar bakın.

![Sohbet örnekleri](tutorial-getting-started-with-signalr/_static/image1.png)

Bölümler:

- [Projesi ayarlayın](#setup)
- [Örnek çalıştırın](#run)
- [Kodu inceleyin](#code)
- [Sonraki adımlar](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Projesi ayarlayın

Bu bölümde, boş bir ASP.NET web uygulaması oluşturmak gösterilmektedir, SignalR ekleyin ve sohbet uygulaması oluşturma.

Önkoşullar:

- Visual Studio 2010 SP1 veya 2012. Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.
- [Microsoft ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Visual Studio 2012 için bu yükleyiciyi Visual Studio için SignalR şablonları dahil olmak üzere yeni ASP.NET özellikleri ekler. Visual Studio 2010 SP1 için bir yükleyici kullanılabilir değildir ancak Kurulum adımlarında açıklandığı gibi SignalR NuGet paketini yükleyerek öğretici tamamlayabilirsiniz.

ASP.NET boş Web uygulaması oluşturmak ve SignalR Kitaplığı eklemek için Visual Studio 2012 aşağıdaki adımları kullanın:

1. Visual Studio'da ASP.NET boş Web uygulaması oluşturun.

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)
2. Açık **Paket Yöneticisi Konsolu** seçerek **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**. Konsol penceresine aşağıdaki komutu girin:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Bu komut SignalR en son sürümünü yükler 1.x.
3. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | Sınıf**. Yeni sınıf **ChatHub**.
4. İçinde **Çözüm Gezgini** betikleri düğümünü genişletin. JQuery ve SignalR için komut dosyası kitaplıkları projede görünür.

    ![Kitaplık Başvurusu](tutorial-getting-started-with-signalr/_static/image3.png)
5. Kodla **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **genel uygulama sınıfı** tıklatıp **Ekle**.

    ![Genel ekleme](tutorial-getting-started-with-signalr/_static/image4.png)
7. Aşağıdakileri ekleyin `using` sağlanan sonra deyimleri `using` Global.asax.cs sınıfı deyimlerinde.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Aşağıdaki kod satırını ekleyin `Application_Start` SignalR hub'ları için varsayılan yol kaydetmek için genel sınıfının yöntemi.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusu, select Html sayfası ve tıklatın **Ekle**.
10. İçinde **Çözüm Gezgini**, az önce oluşturduğunuz HTML sayfası sağ tıklatın ve **Başlangıç Sayfası Ayarla**.
11. HTML sayfası varsayılan kodu aşağıdaki kodla değiştirin.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örnek çalıştırın

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın. HTML sayfası bir tarayıcı örneği ve bir kullanıcı adı için ister yükler.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image5.png)
2. Bir kullanıcı adı girin.
3. Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın. Her tarayıcı örnek benzersiz bir kullanıcı adı girin.
4. Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**. Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcı yorumları yayınlar. Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.

    Aşağıdaki ekran görüntüsünde bir örnek ileti gönderdiğinde, güncelleştirilen üç tarayıcı durumlarda çalışan sohbet uygulama gösterir:

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image6.png)
5. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm. Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir.

    ![Oluşturulan hub komut dosyası](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Kodu inceleyin

SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı. Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır. Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında jQuery komut dosyalarından çağırarak erişim.

İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem. Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.broadcastMessage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için dinamik özellik bu hub'ına bağlı.
- İstemcide bir jQuery işlevi çağırma (aşağıdaki gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

Kod örneğinde HTML sayfası SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir. Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilirsiniz işlevi bildirme ve hub'ına iletileri göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.

Aşağıdaki kod, bir hub için bir proxy bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> JQuery içinde sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur. Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**.


Aşağıdaki komut dosyasında bir geri çağırma işlevini oluşturma kodudur. Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. HTML kodlama, içeriği görüntülemeden önce iki satır isteğe bağlıdır ve kod eklemesini önlemeye yönelik basit bir yol gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir. Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasının düğmesini.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana oluşturmasını sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz. Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.

Örnek uygulamayı Bu öğretici veya diğer SignalR uygulamalarını Internet üzerinden bir barındırma sağlayıcısına dağıtarak sunabilirsiniz. Microsoft, 10 web siteleri için ücretsiz bir ücretsiz web barındırma sunar [Windows Azure deneme sürümü hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Örnek SignalR uygulama dağıtma hakkında bir kılavuz için bkz: [SignalR alma başlatıldı örnek olarak bir Windows Azure Web sitesi yayımlama](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [bir ASP.NET uygulaması için bir Windows Azure Web sitesi dağıtma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Not: WebSocket aktarımı şu anda Windows Azure Web siteleri için desteklenmiyor. Zaman WebSocket taşıma kullanılabilir değil, SignalR kullanan diğer kullanılabilir taşımalar taşımaları bölümünde açıklandığı gibi [SignalR konuya giriş](index.md).)

Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR Project](http://signalr.net)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
