---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SQL Server ile SignalR ölçeğini genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: cd0e3d4bdb4d2eb78e5c41167a17f8673584f654
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751921"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>SQL Server ile SignalR ölçeğini genişletme (SignalR 1.x)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Bu öğreticide, iki ayrı IIS ınstances'ta dağıtılabilen bir SignalR uygulama iletilerini dağıtmak üzere SQL Server'ı kullanır. Bu öğreticide tek test makinesinde çalıştırabilirsiniz ancak tam etkisi almak için iki veya daha fazla sunucu SignalR uygulamayı dağıtmak gerekir. SQL Server sunuculardan biri üzerinde veya ayrılmış ayrı bir sunucuya yüklemeniz gerekir. Başka bir seçenek, Azure üzerinde sanal makineleri kullanarak öğreticiyi çalıştırmaktır.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Önkoşullar

Microsoft SQL Server 2005 veya üzeri. Devre kartına hem Masaüstü hem de sunucu SQL Server sürümlerini destekler. SQL Server Compact Edition veya Azure SQL veritabanı desteklemez. (Uygulamanız Azure üzerinde barındırılıyorsa, bunun yerine Service Bus devre kartına düşünün.)

## <a name="overview"></a>Genel Bakış

Ayrıntılı Öğreticisine aldığımız önce ne yapacağını, hızlı bir genel bakış aşağıda verilmiştir.

1. Yeni bir boş veritabanı oluşturursunuz. Devre kartına gerekli tabloları bu veritabanında oluşturur.
2. Bu NuGet paketlerini uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Veritabanını yapılandırın

Uygulamayı Windows kimlik doğrulaması veya SQL Server kimlik doğrulaması veritabanına erişmek için kullanıp kullanmayacağını karar verin. Ne olursa olsun, veritabanı kullanıcı oturum açmak için şema oluşturma ve tablo oluşturma izni olduğundan emin olun.

Devre kartına kullanmak için yeni bir veritabanı oluşturun. Veritabanı adı verebilirsiniz. Veritabanında tablo oluşturmanız gerekmez; devre kartına gerekli tabloları oluşturur.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Hizmet Aracısı'nı etkinleştirin

Devre kartına veritabanı için hizmet Aracısı etkinleştirme önerilir. Hizmet Aracısı ileti ve daha verimli bir şekilde güncelleştirmelerini devre kartına sağlayan, SQL Server'da queuing için yerel destek sağlar. (Ancak devre kartına Ayrıca hizmet aracısı çalışır.)

Hizmet Aracısı etkin olup olmadığını denetlemek için sorgu **olduğu\_Aracısı\_etkin** sütununda **sys.databases** Katalog görünümü.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Hizmet Aracısı'nı etkinleştirmek için aşağıdaki SQL sorgusunu kullanın:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Bu sorgu görünürse emin olmak için kilitlenme DB'ye bağlı hiç uygulama yok.


İzleme etkinleştirilirse, izlemeleri Ayrıca hizmet Aracısı etkin olup olmadığını gösterir.

## <a name="create-a-signalr-application"></a>Bir SignalR uygulaması oluşturun

Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:

- [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ardından, biz sohbet uygulaması, SQL Server ile ölçeğini genişletme destekleyecek şekilde değiştireceksiniz. İlk olarak, SignalR.SqlServer NuGet paketini projenize ekleyin. Visual Studio'da gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Ardından, Global.asax dosyası açın. Aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Dağıtma ve uygulama çalıştırma

SignalR uygulamayı dağıtmak için Windows Server örneklerinizin hazırlayın.

IIS rolünü ekleyin. WebSocket Protokolü dahil olmak üzere "Uygulama geliştirme" özellikleri içerir.

![](scaleout-with-sql-server/_static/image4.png)

Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) de içerir.

![](scaleout-with-sql-server/_static/image5.png)

**Yükleme Web dağıtımı 3.0.** IIS Yöneticisi'ni çalıştırın, Microsoft Web Platformu'nu yüklemenizi ister veya yapabilecekleriniz [intstaller indirme](https://go.microsoft.com/fwlink/?LinkId=255386). Platform Yükleyicisi'nde, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin

![](scaleout-with-sql-server/_static/image6.png)

Web yönetimi hizmeti çalışıp çalışmadığını denetleyin. Aksi durumda, hizmeti başlatın. (Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)

Son olarak, TCP bağlantı noktası 8172 açın. Bu, Web dağıtımı Aracı'nı kullanan bağlantı noktasıdır.

Geliştirme makinenizde Visual Studio projesinden sunucusuna dağıtmak hazırsınız. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.

Belgeler web dağıtımı hakkında daha fazla ayrıntılı için bkz [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).

İki sunucu için uygulama dağıtıyorsanız, her örnek bir ayrı bir tarayıcı penceresi açın ve her SignalR iletileri diğer aldığınız bakın. (Kuşkusuz, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)

![](scaleout-with-sql-server/_static/image7.png)

Uygulamayı çalıştırdıktan sonra bir veritabanında tabloları, SignalR otomatik olarak oluşturduğu görebilirsiniz:

![](scaleout-with-sql-server/_static/image8.png)

SignalR tablolarını yönetir. Uygulamanızın dağıtıldığı sürece, yok satırları silmek, tabloyu değiştirebilir ve VS.
