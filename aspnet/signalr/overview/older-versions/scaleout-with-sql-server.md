---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: SQL Server ile SignalR genişletme (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 7a589f5c6a5196444c3c616b39f501f3a7512471
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565989"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a>SQL Server ile SignalR genişletme (SignalR 1.x)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

Bu öğreticide, iki ayrı IIS durumlarda dağıtılan bir SignalR uygulaması üzerinden iletilerini dağıtmak için SQL Server kullanır. Bu öğretici bir tek test makinesi üzerinde de çalıştırabilirsiniz, ancak tam etkisi almak için iki veya daha fazla sunucu SignalR uygulamayı dağıtmak gerekir. SQL Server sunuculardan birinde ya da ayrılmış ayrı bir sunucuya yüklemeniz gerekir. Sanal makineleri Azure üzerinde kullanarak öğreticisini çalıştırma başka bir seçenektir.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Önkoşullar

Microsoft SQL Server 2005 veya üzeri. Devre kartına SQL Server'ın masaüstü ve sunucu sürümleri destekler. SQL Server Compact Edition veya Azure SQL Database desteklemez. (Uygulamanızın Azure üzerinde barındırılan, bunun yerine hizmet veri yolu devre kartı düşünün.)

## <a name="overview"></a>Genel Bakış

Biz ayrıntılı öğretici ulaşmadan ne yapacağını Hızlı Bakış aşağıda verilmiştir.

1. Yeni bir boş veritabanı oluşturun. Devre kartına bu veritabanında gerekli tabloları oluşturur.
2. Bu NuGet paketleri uygulamanıza ekleyin: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Bir SignalR uygulaması oluşturun.
4. Devre kartına yapılandırmak için Global.asax için aşağıdaki kodu ekleyin: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Veritabanını yapılandırın

Uygulamayı Windows kimlik doğrulaması veya SQL Server kimlik doğrulaması veritabanına erişmek için kullanıp kullanmayacağını karar verin. Ne olursa olsun, veritabanı kullanıcı oturum açma, şemaları oluşturma ve tabloları oluşturma izni olduğundan emin olun.

Kullanılacak devre kartı için yeni bir veritabanı oluşturun. Veritabanı adı verebilirsiniz. Tüm tabloları veritabanında oluşturmanız gerekmez; devre kartına gerekli tabloları oluşturur.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Hizmet Aracısı etkinleştirme

Hizmet Aracısı devre kartı veritabanı için etkinleştirilmesi önerilir. Hizmet Aracısı ileti ve daha verimli bir şekilde güncelleştirmelerini devre kartı sağlayan, SQL Server'da sıraya alma için yerel destek sağlar. (Ancak devre kartı Ayrıca hizmet aracısı çalışır.)

Service Broker etkin olup olmadığını denetlemek için sorgu **olan\_Aracısı\_etkin** sütununda **sys.databases** Katalog görünümü.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Hizmet Aracısı etkinleştirmek için aşağıdaki SQL sorgusunu kullanın:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Bu sorgu görünürse kilitlenme, emin olmak için Veritabanına bağlı uygulama yok.


İzleme etkinleştirilirse, izlemeleri Ayrıca hizmet Aracısı etkin olup olmadığını gösterir.

## <a name="create-a-signalr-application"></a>Bir SignalR uygulaması oluşturma

Bir SignalR uygulaması ya da bu öğreticileri izleyerek oluşturun:

- [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md)
- [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md)

Ardından, biz SQL Server ile genişletme desteklemek için Sohbet uygulaması değiştireceksiniz. İlk olarak, SignalR.SqlServer NuGet paketini projenize ekleyin. Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Ardından, Global.asax dosyası açın. Aşağıdaki kodu ekleyin **uygulama\_Başlat** yöntemi:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Dağıtma ve uygulamayı çalıştırma

SignalR uygulamayı dağıtmak için Windows Server örneklerinizi hazırlayın.

IIS rolü ekleyin. WebSocket Protokolü dahil olmak üzere, "Uygulama geliştirme" özellikleri bulunur.

![](scaleout-with-sql-server/_static/image4.png)

Ayrıca Yönetim Hizmeti ("Yönetim Araçları" altında listelenen) içerir.

![](scaleout-with-sql-server/_static/image5.png)

**Yükleme Web dağıtımı 3.0.** IIS Yöneticisi'ni çalıştırdığınızda, Microsoft Web Platformu yüklemenizi ister veya yapabilecekleriniz [intstaller karşıdan](https://go.microsoft.com/fwlink/?LinkId=255386). Platformu yükleyicisi, Web dağıtımı için arama ve Web dağıtımı 3.0 yükleyin

![](scaleout-with-sql-server/_static/image6.png)

Web yönetimi hizmeti çalışıp çalışmadığını denetleyin. Aksi durumda, hizmeti başlatın. (Web yönetimi hizmeti Windows Hizmetleri listede görmüyorsanız, IIS rolü eklendiğinde yönetim Hizmeti'nin yüklü emin olun.)

Son olarak, TCP bağlantı noktası 8172 açın. Bu Web Dağıtım Aracı'nı kullanır bağlantı noktasıdır.

Artık geliştirme makinenizden Visual Studio projesi sunucuya dağıtmaya hazır olursunuz. Çözüm Gezgini'nde çözüme sağ tıklayın ve **Yayımla**.

Daha ayrıntılı web dağıtımı ile ilgili belgeler için bkz: [Visual Studio ve ASP.NET için Web dağıtımı içerik haritası](../../../whitepapers/aspnet-web-deployment-content-map.md).

İki sunucu için uygulama dağıtıyorsanız, her örneği ayrı bir tarayıcı penceresinde açın ve her SignalR iletileri diğer aldığınız bakın. (Elbette, bir üretim ortamında, iki sunucu bir yük dengeleyicinin arkasına sit.)

![](scaleout-with-sql-server/_static/image7.png)

Uygulamayı çalıştırdıktan sonra veritabanında tablolarda, SignalR otomatik olarak oluşturdu görebilirsiniz:

![](scaleout-with-sql-server/_static/image8.png)

SignalR tabloları yönetir. Uygulamanızı dağıttınız sürece yok satırları silmek, tabloyu değiştirerek ve benzeri.
