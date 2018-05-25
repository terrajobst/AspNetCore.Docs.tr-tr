---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Öğretici: 2 SignalR ile çalışmaya başlama | Microsoft Docs'
author: pfletcher
description: Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. Boş bir ASP.NET web uygulamasına SignalR ekleyebilir ve bir HTML pa oluştur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a>Öğretici: 2 SignalR ile çalışmaya başlama
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> Bu öğreticide SignalR kullanarak gerçek zamanlı bir sohbet uygulaması oluşturma işlemi gösterilir. Boş bir ASP.NET web uygulamasına SignalR ekleyebilir ve göndermek ve iletileri görüntülemek için bir HTML sayfası oluşturun. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
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

Bu öğretici, bir basit tarayıcı tabanlı sohbet uygulaması oluşturmak nasıl yapıldığını göstererek SignalR geliştirme sunmaktadır. Boş bir ASP.NET web uygulamasına SignalR Kitaplığı eklemek, istemcilere ileti göndermek için bir hub sınıf oluşturun ve sohbet ileti gönderme ve alma kullanıcıların olanak sağlayan bir HTML sayfası oluşturun. MVC 5'te bir sohbet uygulaması oluşturmak MVC görünümü kullanarak gösteren benzer bir öğretici için bkz: [SignalR 2 ve MVC 5 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Bu öğretici SignalR uygulamalarının sürüm 2 nasıl oluşturulacağını gösterir. SignalR arasındaki değişiklikleri hakkında ayrıntılı bilgi için bkz: 1.x ve 2, [yükseltme SignalR 1.x projeleri](../releases/upgrading-signalr-1x-projects-to-20.md) ve [Visual Studio 2013 sürüm notları](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR, Canlı kullanıcı etkileşimi veya gerçek zamanlı veri güncelleştirmeleri gerektiren web uygulamaları oluşturmak için bir açık kaynak .NET kitaplığıdır. Sosyal uygulamalar, çok kullanıcılı oyunlar, iş işbirliği ve haberler, hava durumu veya finansal güncelleştirme uygulamaları örnek olarak verilebilir. Bunlar genellikle gerçek zamanlı uygulamaları olarak adlandırılır.

SignalR gerçek zamanlı uygulamaları oluşturma işlemini basitleştirir. Bir ASP.NET server kitaplığı ve istemci-sunucu bağlantıları yönetmek ve içerik güncelleştirmelerini istemcilere anında kolaylaştırmak için JavaScript istemci kitaplığını içerir. Gerçek zamanlı işlevselliği sağlamak için mevcut bir ASP.NET uygulamasını için SignalR kitaplığı ekleyebilirsiniz.

Öğretici aşağıdaki SignalR geliştirme görevleri gösterir:

- SignalR kitaplığı, bir ASP.NET web uygulamasına ekleniyor.
- İçeriği istemcilere göndermek için bir hub sınıfı oluşturma.
- Uygulamayı yapılandırmak için OWIN başlangıç sınıfı oluşturma.
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

Bu bölümde Visual Studio 2013 ve SignalR sürüm 2 boş bir ASP.NET web uygulaması oluşturmak için nasıl kullanılacağı gösterilmiştir SignalR ekleyin ve sohbet uygulaması oluşturma.

Önkoşullar:

- Visual Studio 2013. Visual Studio yoksa bkz [ASP.NET indirmeleri](https://www.asp.net/downloads) ücretsiz Visual Studio 2013 Express geliştirme aracı alınamıyor.

ASP.NET boş Web uygulaması oluşturmak ve SignalR Kitaplığı eklemek için Visual Studio 2013 aşağıdaki adımları kullanın:

1. Visual Studio'da ASP.NET Web uygulaması oluşturun.

    ![Web oluşturulamıyor](tutorial-getting-started-with-signalr/_static/image2.png)
2. İçinde **yeni ASP.NET projesi** penceresinde, bırakın **boş** 'ı tıklatın ve seçili **proje oluştur**.

    ![Boş web oluşturma](tutorial-getting-started-with-signalr/_static/image3.png)
3. İçinde **Çözüm Gezgini**, projeye sağ tıklayın, seçin **Ekle | SignalR hub'ı sınıfı (v2)**. Sınıf adını **ChatHub.cs** ve bunu projeye ekleyin. Bu adım oluşturur **ChatHub** sınıfı ve bir dizi komut dosyaları ve Signalr'yi destekleyen derleme başvurularını projeye ekler.

    > [!NOTE]
    > Açarak projeye SignalR ekleyebilirsiniz **Araçlar | Kitaplık Paket Yöneticisi | Paket Yöneticisi Konsolu** ve bir komut çalıştırma:

    `install-package Microsoft.AspNet.SignalR`

    SignalR eklemek için konsol kullanırsanız, SignalR hub'ı sınıf SignalR ekledikten sonra ayrı bir adım olarak oluşturun.

    > [!NOTE]
    > Visual Studio 2012 kullanıyorsanız **SignalR hub'ı sınıfı (v2)** şablonu kullanılabilir olmaz. Bir düz ekleyebilirsiniz **sınıfı** adlı `ChatHub` yerine.
4. İçinde **Çözüm Gezgini**, komut dosyaları düğümünü genişletin. JQuery ve SignalR için komut dosyası kitaplıkları projede görünür.
5. Kod yeni değiştirin **ChatHub** aşağıdaki kodla sınıfı.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | OWIN başlangıç sınıfı**. Yeni sınıf `Startup` ve Tamam'ı tıklatın.

    > [!NOTE]
    > Visual Studio 2012 kullanıyorsanız **OWIN başlangıç sınıfı** şablonu kullanılabilir olmaz. Bir düz ekleyebilirsiniz **sınıfı** adlı `Startup` yerine.
7. Yeni başlangıç sınıfı içeriğini aşağıdaki gibi değiştirin.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve ardından **Ekle | HTML sayfası**. Yeni sayfa adı `index.html`.
    >[!NOTE]
    >Sürüm numaraları JQuery ve SignalR kitaplıklarına başvurular için değiştirmeniz gerekebilir
9. İçinde **Çözüm Gezgini**, az önce oluşturduğunuz HTML sayfası sağ tıklatın ve **Başlangıç Sayfası Ayarla**.
10. HTML sayfası varsayılan kodu aşağıdaki kodla değiştirin.

    > [!NOTE]
    > SignalR betikler daha sonraki bir sürümü Paket Yöneticisi tarafından yüklenebilir. Aşağıdaki komut dosyası başvuruları (bunlar bir hub'ı eklemek yerine NuGet kullanarak SignalR eklediyseniz farklı olacaktır.) proje komut dosyalarında sürümlerine karşılık geldiğinden emin olun

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Tümünü Kaydet** projesi için.

<a id="run"></a>

## <a name="run-the-sample"></a>Örnek çalıştırın

1. Projeyi hata ayıklama modunda çalıştırmak için F5 tuşuna basın. HTML sayfası bir tarayıcı örneği ve bir kullanıcı adı için ister yükler.

    ![Kullanıcı adı girin](tutorial-getting-started-with-signalr/_static/image4.png)
2. Bir kullanıcı adı girin.
3. Tarayıcının adres satırından URL'yi kopyalayın ve iki daha fazla tarayıcı örnekleri açmak için kullanın. Her tarayıcı örnek benzersiz bir kullanıcı adı girin.
4. Her tarayıcı örnek bir açıklama ekleyin ve **Gönder**. Açıklamaları tüm tarayıcı örnekleri görüntülemelidir.

    > [!NOTE]
    > Bu basit sohbet uygulama sunucusunda tartışma bağlam korumaz. Hub'ın tüm geçerli kullanıcı yorumları yayınlar. Sohbet daha sonra katılmak kullanıcılar zamandan eklenen iletilerini bunlar katılma görürsünüz.

    Aşağıdaki ekran görüntüsünde bir örnek ileti gönderdiğinde, güncelleştirilen üç tarayıcı durumlarda çalışan sohbet uygulama gösterir:

    ![Sohbet tarayıcılar](tutorial-getting-started-with-signalr/_static/image5.png)
5. İçinde **Çözüm Gezgini**, inceleme **betik belgelerini** çalışan uygulama için düğüm. Adlı bir komut dosyası **hub** , SignalR kitaplık çalışma zamanında dinamik olarak oluşturur. Bu dosya, jQuery betik sunucu tarafı kodu arasında iletişim yönetir.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Kodu inceleyin

SignalR sohbet uygulaması iki temel SignalR geliştirme görevlerini gösterir: sunucu üzerindeki ana koordinasyon nesnesi olarak bir hub'ı oluşturma ve ileti gönderme ve alma için SignalR jQuery kitaplığı kullanma.

### <a name="signalr-hubs"></a>SignalR hub'ları

Kod örneğinde **ChatHub** sınıfı türer **Microsoft.AspNet.SignalR.Hub** sınıfı. Türetme **Hub** bir SignalR uygulaması oluşturmak için kullanışlı bir yol bir sınıftır. Genel yöntemler hub sınıfınız oluşturun ve sonra bu yöntemler bir web sayfasında komut dosyalarından çağırarak erişim.

İstemciler sohbet kodda çağrısı **ChatHub.Send** yeni bir ileti göndermek için yöntem. Hub sırayla iletiyi tüm istemcilere çağırarak gönderir **Clients.All.broadcastMessage**.

**Gönder** yöntemi birkaç hub kavramları göstermektedir:

- Böylece istemciler bunları çağırabilirsiniz genel yöntemler bir hub'ına bildirin.
- Kullanım **Microsoft.AspNet.SignalR.Hub.Clients** tüm istemciler erişmek için dinamik özellik bu hub'ına bağlı.
- İstemcide bir işlevi çağırmak (gibi `broadcastMessage` işlevi) istemcilerini güncelleştirmek için.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR ve jQuery

Kod örneğinde HTML sayfası SignalR jQuery kitaplığı bir SignalR hub ile iletişim kurmak için nasıl kullanılacağını gösterir. Kodda önemli görevleri istemcilere anında içeriği için sunucu çağırabilirsiniz işlevi bildirme ve hub'ına iletileri göndermek için bir bağlantı başlatma hub başvurmak için bir proxy bildirdiğiniz.

Aşağıdaki kod, bir hub proxy için bir başvuru bildirir.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> JavaScript'te sunucu sınıfı ve üyelerini içinde ortası büyük başvurudur. Kod örneği C# başvuran **ChatHub** JavaScript sınıfında **chatHub**.


Aşağıdaki komut dosyasında bir geri çağırma işlevini oluşturma kodudur. Sunucudaki hub sınıfı içerik güncelleştirmeleri her bir istemciye göndermek için bu işlevi çağırır. HTML kodlama, içeriği görüntülemeden önce iki satır isteğe bağlıdır ve kod eklemesini önlemeye yönelik basit bir yol gösterir.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Aşağıdaki kod, hub ile bir bağlantı açmak gösterilmiştir. Kod bağlantı başlar ve üzerinde click olayını işlemek için bir işlev geçirir **Gönder** HTML sayfasının düğmesini.

> [!NOTE]
> Bu yaklaşım, olay işleyici yürütülmeden önce bağlantı kurulana sağlar.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Sonraki Adımlar

SignalR gerçek zamanlı web uygulamaları oluşturmak için bir çerçeve olduğunu öğrendiniz. Birkaç SignalR geliştirme görevleri de öğrenilen: bir ASP.NET uygulaması için SignalR ekleme, hub sınıfın nasıl oluşturulacağı ve hub'dan ileti alıp göndermek nasıl.

Örnek SignalR uygulamayı Azure'a dağıtma hakkında bir kılavuz için bkz: [SignalR kullanarak Azure App Service'te Web uygulamalarını](../deployment/using-signalr-with-azure-web-sites.md). Windows Azure Web sitesi için bir Visual Studio web projesi dağıtma hakkında ayrıntılı bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Daha gelişmiş SignalR gelişmeler kavramları hakkında bilgi için SignalR kaynak kodu ve kaynaklar için aşağıdaki siteleri ziyaret edin:

- [SignalR Project](http://signalr.net)
- [SignalR Github ve örnekler](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
