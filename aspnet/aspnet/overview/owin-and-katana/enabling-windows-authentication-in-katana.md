---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Katana'da Windows kimlik doğrulamasını etkinleştirme | Microsoft Docs
author: MikeWasson
description: "Bu makalede katana'da Windows kimlik doğrulamasının nasıl etkinleştirileceği gösterilmektedir. İki senaryoyu da kapsamaktadır: IIS konağa Katana ve Kat barındırma için HttpListener kullanarak..."
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 0aa578020a1f02fa68c74e758014c642219b4265
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753156"
---
<a name="enabling-windows-authentication-in-katana"></a>Katana'da Windows kimlik doğrulamasını etkinleştirme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Bu makalede katana'da Windows kimlik doğrulamasının nasıl etkinleştirileceği gösterilmektedir. İki senaryoyu da kapsamaktadır: IIS konağa Katana ve Katana özel bir işlemde kendiliğinden barındırmak için HttpListener kullanarak. Bu makalede gözden geçirme için teşekkür ederiz Barry Dorrans, David Matson ve Chris Ross.


Katana Microsoft'un uygulaması olan [OWIN](http://owin.org/), .NET için açık Web arabirimi. OWIN ve Katana giriş edinebilirsiniz [burada](an-overview-of-project-katana.md). OWIN mimari birkaç katmana sahiptir:

- Ana bilgisayarı: OWIN ardışık düzenini çalıştığı işlemin yönetir.
- Sunucu: bir ağ yuva açar ve isteklerini dinler.
- Ara yazılım: HTTP istek ve yanıt işler.

Katana şu anda iki sunucu, Windows tümleşik kimlik doğrulaması ikisi için de destek sağlar:

- **Microsoft.Owin.Host.SystemWeb**. IIS ile ASP.NET ardışık düzenini kullanır.
- **Microsoft.Owin.Host.HttpListener**. Kullanan [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Bu sunucu şu anda kendi kendine barındırma olduğunda varsayılan seçenek olan Katana.

> [!NOTE]
> Bu işlevselliği sunucular kullanılabilir olduğundan Katana şu anda OWIN ara yazılımı için Windows kimlik doğrulaması sağlamaz.


## <a name="windows-authentication-in-iis"></a>IIS Windows kimlik doğrulaması

Microsoft.Owin.Host.SystemWeb kullanarak, yalnızca IIS Windows kimlik doğrulamasını etkinleştirebilirsiniz.

"ASP.NET boş Web uygulaması" proje şablonunu kullanarak yeni bir ASP.NET uygulaması oluşturarak başlayalım.

![](enabling-windows-authentication-in-katana/_static/image1.png)

Ardından, NuGet paketlerini ekleyin. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Şimdi adlı bir sınıf ekleyin `Startup` aşağıdaki kod ile:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Tüm OWIN, IIS üzerinde çalışan bir "Hello world" uygulaması oluşturmak için gereken budur. Uygulamada hata ayıklamak için F5 tuşuna basın. "Hello World!" görmeniz gerekir tarayıcı penceresinde.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Ardından, IIS Express Windows kimlik doğrulaması etkinleştiririz. Gelen **görünümü** menüsünde **özellikleri**. Proje özelliklerini görüntülemek için Çözüm Gezgini'nde proje adının üzerine tıklayın.

İçinde **özellikleri** penceresinde **anonim kimlik doğrulaması** için **devre dışı bırakılmış** ayarlayıp **Windows kimlik doğrulaması** için  **Etkin**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Visual Studio'dan uygulamayı çalıştırdığınızda, IIS Express kullanıcının Windows kimlik bilgilerini gerektirir. Bunu kullanarak görmek [Fiddler](http://fiddler2.com/home) veya başka bir HTTP hata ayıklama aracı. HTTP yanıt örneği burada verilmiştir:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Bu yanıt WWW-Authenticate üstbilgi sunucu desteklediğini göstermek [anlaş](http://www.ietf.org/rfc/rfc4559.txt) protokolü, Kerberos veya NTLM kullanır.

Bir sunucuya uygulamayı dağıttığınızda, daha sonra izleyin [adımları](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) o sunucuda IIS Windows kimlik doğrulamasını etkinleştirmek için.

## <a name="windows-authentication-in-httplistener"></a>HttpListener Windows kimlik doğrulaması

Katana barındırma için Microsoft.Owin.Host.HttpListener kullanıyorsanız, doğrudan Windows kimlik doğrulamasını etkinleştirebilirsiniz **HttpListener** örneği.

İlk olarak, yeni bir konsol uygulaması oluşturun. Ardından, NuGet paketlerini ekleyin. Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Şimdi adlı bir sınıf ekleyin `Startup` aşağıdaki kod ile:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Bu sınıf önce aynı "Hello world" örnek uygular, ancak ayrıca Windows kimlik doğrulaması kimlik doğrulama düzeni ayarlar.

İçinde `Main` işlev, OWIN ardışık düzenini başlatın:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Uygulamayı Windows kimlik doğrulaması kullandığını doğrulayın Fiddler'da bir istek gönderebilirsiniz:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>İlgili Konular

[Project Katana’ya Genel Bakış](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[MVC 5 OWIN form kimlik doğrulamasını anlama](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
