---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Öğretici: SignalR 2 ile çalışmaya başlama | Microsoft Docs'
author: pfletcher
description: Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. SignalR için boş bir ASP.NET web uygulamasına ekleme ve bir HTML pa oluştur...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3b06e7d0a602e061112adbceba92276f836f6311
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287344"
---
<a name="tutorial-getting-started-with-signalr-2"></a>Öğretici: SignalR 2 ile Çalışmaya Başlama
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Projeyi yükle](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. SignalR için boş bir ASP.NET web uygulamasına ekleme ve gönderin ve iletileri görüntülemek için bir HTML sayfası oluşturun. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
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

Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme tanıtır. SignalR kitaplık için boş bir ASP.NET web uygulamasına eklemek, istemcilere ileti göndermek için hub sınıfı oluşturun ve sohbet iletileri gönderip kullanıcıların olanak sağlayan bir HTML sayfası oluşturmak. MVC 5'te bir sohbet uygulaması oluşturma işlemini kullanarak bir MVC görünümü gösteren benzer bir öğretici için bkz. [SignalR 2 ve MVC 5 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Bu öğreticide, sürüm 2 SignalR uygulamalarını oluşturma gösterilmektedir. SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için 1.x ve 2 ' nin bkz [yükseltme SignalR 1.x projelerini](../releases/upgrading-signalr-1x-projects-to-20.md) ve [Visual Studio 2013 sürüm notları](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmaya yönelik bir açık kaynak .NET Kitaplığı ' dir. Sosyal uygulamalar, çok kullanıcılı bir oyun, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları verilebilir. Bunlar genellikle gerçek zamanlı uygulamalar olarak adlandırılır.

SignalR, gerçek zamanlı uygulamalar oluşturma işlemini basitleştirir. Bu, bir ASP.NET sunucu kitaplığı ve istemci-sunucu bağlantılarını yönetme ve içerik güncelleştirmelerini istemcilere göndermek daha kolay hale getirmek için bir JavaScript istemci kitaplığı içerir. Gerçek zamanlı işlevsellik sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığa ekleyebilirsiniz.

Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:

- SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.
- İçeriği istemcilere göndermek için bir hub sınıf oluşturuluyor.
- Uygulamayı yapılandırmak için OWIN başlangıç sınıfı oluşturuluyor.
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

Bu bölümde Visual Studio 2013 ve SignalR sürüm 2 boş bir ASP.NET web uygulaması oluşturmak için nasıl kullanılacağını gösterir, SignalR ekleyin ve sohbet uygulaması oluşturma.

Önkoşullar:

- Visual Studio 2013. Visual Studio yoksa bkz [ASP.NET indirir](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.

ASP.NET boş Web uygulaması oluşturma ve SignalR Kitaplığı eklemek için Visual Studio 2013'ün aşağıdaki adımları kullanın:

1. Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.

    ![Web oluşturma](tutorial-getting-started-with-signalr/_static/image2.png)
2. İçinde **yeni ASP.NET projesi** penceresinde bırakın **boş** tıklatın ve seçili **proje oluştur**.

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image3.png)
3. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, **Ekle | SignalR Hub sınıfı (v2)**. Sınıf adı **ChatHub.cs** ve projeye ekleyin. Bu adımda oluşturulur **ChatHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen bir bütünleştirilmiş kod başvuruları projeye ekler.

    > [!NOTE]
    > Açarak bir projeye SignalR ekleyebilirsiniz **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu** ve bir komutu çalıştırılıyor:

    `install-package Microsoft.AspNet.SignalR`

    SignalR eklemek için konsolunu kullanırsanız, SignalR hub sınıfı SignalR ekledikten sonra ayrı bir adım olarak oluşturun.

    > [!NOTE]
    > Visual Studio 2012 kullanıyorsanız **SignalR Hub sınıfı (v2)** şablonu kullanılamaz. Bir düz ekleyebilirsiniz **sınıfı** adlı `ChatHub` yerine.
4. İçinde **Çözüm Gezgini**, betikleri düğümünü genişletin. JQuery ve SignalR için betik kitaplıkları projesinde görünür.
5. Yeni bir kodu **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | OWIN başlangıç sınıfı**. Yeni bir sınıf adı `Startup` ve Tamam'a tıklayın.

    > [!NOTE]
    > Visual Studio 2012 kullanıyorsanız **OWIN başlangıç sınıfı** şablonu kullanılamaz. Bir düz ekleyebilirsiniz **sınıfı** adlı `Startup` yerine.
7. Yeni başlangıç sınıfı içeriğini şu şekilde değiştirin.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. İçinde **Çözüm Gezgini**, projeyi sağ tıklatın ve ardından **Ekle | HTML sayfası**. Yeni sayfa adı `index.html`.
    >[!NOTE]
    >JQuery ve SignalR kitaplıklarına başvurular için sürüm numaraları değiştirmeniz gerekebilir
9. İçinde **Çözüm Gezgini**, yeni oluşturduğunuz HTML sayfasının sağ tıklatıp **Başlangıç Sayfası Ayarla**.
10. HTML sayfasındaki varsayılan kodu aşağıdaki kodla değiştirin.

    > [!NOTE]
    > SignalR betikleri daha sonraki bir sürümünü Paket Yöneticisi tarafından yüklenebilir. Aşağıdaki betik başvurularını komut dosyalarını (bunlar SignalR hub'ı eklemek yerine NuGet kullanarak eklediyseniz farklı olacaktır.) projede sürümlerine karşılık geldiğini doğrulayın

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın. Bir tarayıcı örneğinde ve bir kullanıcı adı için istemleri HTML sayfasını yükler.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image4.png)
2. Bir kullanıcı adı girin.
3. Tarayıcının adres satırından URL'sini kopyalayın ve iki daha fazla tarayıcı örneği açmak için kullanın. Her bir tarayıcı örneğinde benzersiz bir kullanıcı adı girin.
4. Her bir tarayıcı örneğinde bir açıklama ekleyin ve **Gönder**. Açıklamalar, hepsinin tarayıcı görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulaması sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcılar yorum yayınlar. Sohbet daha sonra katılmak kullanıcıların zamandan eklenen iletileri katılmaları görürsünüz.

    Aşağıdaki ekran görüntüsünde, çalışan bir örnek ileti gönderdiğinde tümü güncelleştirilir üç tarayıcı durumlarda sohbet uygulaması gösterilmektedir:

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image5.png)
5. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama düğümü. Adlı bir betik dosyası **hubs** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişimi yönetir.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Kod İnceleme

SignalR sohbet uygulaması iki temel SignalR geliştirme görevleri gösterir: sunucunun ana koordinasyon nesne olarak bir hub'ı oluşturma ve ileti göndermek ve almak için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıf türetilir **Microsoft.AspNet.SignalR.Hub** sınıfı. Öğesinden türetme **Hub** SignalR uygulama oluşturmak için kullanışlı bir yöntem bir sınıftır. Hub sınıfınıza genel yöntemleri oluşturun ve bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişin.

İstemciler sohbet kodda çağrı **ChatHub.Send** yeni bir ileti göndermek için yöntemi. Hub sırayla ileti tüm istemcilere çağırarak gönderen **Clients.All.broadcastMessage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- İstemciler, bunları çağırabilirsiniz genel yöntemleri bir hub'da bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemcilere erişmek için dinamik özellik bu hub'a bağlı.
- İstemcide bir işlevi çağırmayı (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

Kod örneği HTML sayfasındaki bir SignalR hub'ı ile iletişim kurmak için SignalR jQuery kitaplığı kullanmayı gösterir. Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilen bir işlevi bildirmek ve hub'ına ileti göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.

Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> JavaScript'te, sunucu sınıfını ve üyelerini ortası büyük harf başvurudur. Kod örneği C# başvuran **ChatHub** JavaScript olarak sınıfında **chatHub**.


Aşağıdaki betikte bir geri çağırma işlevini nasıl oluşturacağınız kodudur. Sunucudaki hub sınıfına içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. HTML kodlama, içerik görüntülemeden önce aşağıdaki iki satırı isteğe bağlıdır ve kod eklemesini engellemek için basit bir yol gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Aşağıdaki kod hub'ı ile bir bağlantı açmak nasıl gösterir. Kod bağlantı başlar ve ardından üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasındaki düğmesi.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulur sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmaya yönelik bir çerçeve olduğunu öğrendiniz. Ayrıca birkaç SignalR geliştirme görevlerini öğrendiniz: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfı oluşturma ve hub'ından iletiler alan nasıl.

Örnek SignalR uygulamayı Azure'a dağıtma hakkında kılavuz için bkz. [Azure App service'taki Web Apps ile SignalR kullanarak](../deployment/using-signalr-with-azure-web-sites.md). Visual Studio web projesini bir Windows Azure Web sitesine dağıtma hakkında ayrıntılı bilgi için bkz. [Azure App Service'te ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Daha gelişmiş SignalR gelişmeleri kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR projesi](http://signalr.net)
- [SignalR Github ve örnekleri](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
