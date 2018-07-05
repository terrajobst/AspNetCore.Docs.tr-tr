---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR ile çalışmaya başlama 1.x | Microsoft Docs'
author: pfletcher
description: ASP.NET SignalR, bir HTML sayfasında bir gerçek zamanlı bir sohbet uygulaması oluşturmak için kullanın.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3cfeb95bcaa984de5cff246173ad03e2a774fc0c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402028"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Öğretici: SignalR ile çalışmaya başlama 1.x
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. SignalR için boş bir ASP.NET web uygulamasına ekleme ve gönderin ve iletileri görüntülemek için bir HTML sayfası oluşturun.


## <a name="overview"></a>Genel Bakış

Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme tanıtır. SignalR kitaplık için boş bir ASP.NET web uygulamasına eklemek, istemcilere ileti göndermek için hub sınıfı oluşturun ve sohbet iletileri gönderip kullanıcıların olanak sağlayan bir HTML sayfası oluşturmak. Bir MVC görünümü kullanarak MVC 4'te bir sohbet uygulaması oluşturma işlemini gösteren benzer bir öğretici için bkz. [SignalR ve MVC 4 ile çalışmaya başlama](index.md).

> [!NOTE]
> Bu öğreticide SignalR sürüm (1.x) sürümünü kullanır. SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için bkz: 1.x ve 2.0 [yükseltme SignalR 1.x projelerini](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmaya yönelik bir açık kaynak .NET Kitaplığı ' dir. Sosyal uygulamalar, çok kullanıcılı bir oyun, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları verilebilir. Bunlar genellikle gerçek zamanlı uygulamalar olarak adlandırılır.

SignalR, gerçek zamanlı uygulamalar oluşturma işlemini basitleştirir. Bu, bir ASP.NET sunucu kitaplığı ve istemci-sunucu bağlantılarını yönetme ve içerik güncelleştirmelerini istemcilere göndermek daha kolay hale getirmek için bir JavaScript istemci kitaplığı içerir. Gerçek zamanlı işlevsellik sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığa ekleyebilirsiniz.

Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:

- SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.
- İçeriği istemcilere göndermek için bir hub sınıf oluşturuluyor.
- İleti göndermek ve hub'ından güncelleştirmeleri görüntülemek için bir web sayfasında SignalR jQuery kitaplığı kullanıyor.

Aşağıdaki ekran görüntüsünde, bir tarayıcıda çalışan sohbet uygulaması gösterir. Her yeni kullanıcı, yorumlarınızı ve kullanıcı sohbet katıldıktan sonra eklenen yorumlara bakın.

![Sohbet örnekleri](tutorial-getting-started-with-signalr/_static/image1.png)

Bölümler:

- [Projesi kurun](#setup)
- [Örneği çalıştırma](#run)
- [Kod İnceleme](#code)
- [Sonraki adımlar](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Projesi kurun

Bu bölümde, boş bir ASP.NET web uygulaması oluşturma işlemi gösterilmektedir, SignalR ekleyin ve sohbet uygulaması oluşturma.

Önkoşullar:

- Visual Studio 2010 SP1 veya 2012. Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2012 Express geliştirme aracı alınamıyor.
- [Microsoft ASP.NET ve Web Araçları 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Visual Studio 2012 için Visual Studio için SignalR şablonları dahil olmak üzere yeni ASP.NET özellikleri bu yükleyiciyi ekler. Visual Studio 2010 SP1 için bir yükleyici kullanılamaz ancak SignalR NuGet paketini yükleyerek Kurulum adımlarında açıklandığı gibi öğreticiyi tamamlayabilirsiniz.

ASP.NET boş Web uygulaması oluşturma ve SignalR Kitaplığı eklemek için Visual Studio 2012 aşağıdaki adımları kullanın:

1. Visual Studio'da ASP.NET boş Web uygulaması oluşturun.

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)
2. Açık **Paket Yöneticisi Konsolu** seçerek **araçları | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu**. Konsol penceresine aşağıdaki komutu girin:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Bu komut SignalR en son sürümünü yükler 1.x.
3. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | Sınıf**. Yeni bir sınıf adı **ChatHub**.
4. İçinde **Çözüm Gezgini** betikleri düğümünü genişletin. JQuery ve SignalR için betik kitaplıkları projesinde görünür.

    ![Kitaplık başvuruları](tutorial-getting-started-with-signalr/_static/image3.png)
5. Değiştirin **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **genel uygulama sınıfı** tıklatıp **Ekle**.

    ![Genel ekleme](tutorial-getting-started-with-signalr/_static/image4.png)
7. Aşağıdaki `using` deyimleri sağlanan sonra `using` deyimlerinde Global.asax.cs sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Aşağıdaki kod satırını ekleyin `Application_Start` SignalR hub'ları için varsayılan yolu kaydetmek için genel sınıfının yöntemi.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | Yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim, select Html sayfası ve tıklatın **Ekle**.
10. İçinde **Çözüm Gezgini**, yeni oluşturduğunuz HTML sayfasının sağ tıklatıp **Başlangıç Sayfası Ayarla**.
11. HTML sayfasındaki varsayılan kodu aşağıdaki kodla değiştirin.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın. Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri HTML sayfasını yükler.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image5.png)
2. Bir kullanıcı adı girin.
3. Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın. Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.
4. Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**. Açıklamalar, hepsinin tarayıcı görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcılar yorum yayınlar. Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.

    Aşağıdaki ekran görüntüsünde, çalışan bir örnek ileti gönderdiğinde tümü güncelleştirilir üç tarayıcı durumlarda sohbet uygulaması gösterilmektedir:

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image6.png)
5. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü. Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.

    ![Oluşturulan hub betiği](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Kod İnceleme

SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı. Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır. Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında jQuery betiklerden çağırarak erişin.

İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi. Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.broadcastMessage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için dinamik özellik bu hub'a bağlı.
- İstemcide jQuery işlev çağırma (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

Kod örneği HTML sayfasındaki bir SignalR hub'ı ile iletişim kurmak için SignalR jQuery kitaplığı kullanmayı gösterir. Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilen bir işlevi bildirmek ve hub'ına ileti göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.

Aşağıdaki kod, bir hub için bir proxy bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> JQuery içinde sunucu sınıfını ve üyelerini ortası büyük harf başvurudur. Kod örneği C# başvuran **ChatHub** jQuery sınıfında **chatHub**.


Aşağıdaki betikte bir geri çağırma işlevini nasıl oluşturacağınız kodudur. Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. HTML kodlama, içerik görüntülemeden önce aşağıdaki iki satırı isteğe bağlıdır ve kod eklemesini engellemek için basit bir yol gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir. Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasındaki düğmesi.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur oluşturmasını sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz. Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.

Örnek uygulamayı Bu öğretici veya diğer SignalR uygulamalarında kullanılabilir Internet üzerinden bir barındırma sağlayıcısına dağıtarak yapabilirsiniz. Microsoft'un sunduğu ücretsiz olarak 10 adede kadar web siteleri için ücretsiz bir web barındırma [Windows Azure deneme hesabı](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Örnek SignalR uygulama dağıtma hakkında kılavuz için bkz. [SignalR alma çalışmaya örnek olarak bir Windows Azure Web sitesi yayımlama](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [bir ASP.NET uygulaması için bir Windows Azure Web sitesi dağıtma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Not: WebSocket taşıma için Windows Azure Web siteleri şu anda desteklenmiyor. Zaman WebSocket aktarım bulunmuyorsa, SignalR taşımalar bölümünde açıklandığı gibi diğer kullanılabilir aktarımları kullanır [SignalR konusuna giriş](index.md).)

Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
