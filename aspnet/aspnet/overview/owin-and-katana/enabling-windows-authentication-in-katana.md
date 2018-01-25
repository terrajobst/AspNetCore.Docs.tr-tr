---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Katana Windows kimlik doğrulamasını etkinleştirme | Microsoft Docs"
author: MikeWasson
description: "Bu makalede, Katana Windows kimlik doğrulamasını etkinleştirmek gösterilmiştir. İki senaryo kapsar: IIS Katana konağa kullanma ve Kat Self barındırmak için HttpListener kullanma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a>Katana Windows kimlik doğrulamasını etkinleştirme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Bu makalede, Katana Windows kimlik doğrulamasını etkinleştirmek gösterilmiştir. İki senaryo kapsar: IIS Katana konağa kullanma ve HttpListener Katana özel bir işlem olarak kendi kendine barındırmak için kullanma. Bu makalede gözden geçirme için teşekkür ederiz Hakan Dorrans, David Matson ve Chris fillerin.


Katana uygulamasıdır Microsoft'un [OWIN](http://owin.org/), .NET için açık Web arabirimi. OWIN ve Katana giriş okuyabilirsiniz [burada](an-overview-of-project-katana.md). OWIN mimarisi birkaç katmana sahiptir:

- Ana bilgisayarı: OWIN ardışık düzenini çalıştığı sürecini yönetir.
- Sunucu: bir ağ yuva açar ve isteklerini dinler.
- Ara yazılım: HTTP istek ve yanıt işler.

Katana her ikisi de Windows tümleşik kimlik doğrulamasını destekleyen iki sunucu şu anda sağlar:

- **Microsoft.Owin.Host.SystemWeb**. IIS ile ASP.NET ardışık düzenini kullanır.
- **Microsoft.Owin.Host.HttpListener**. Kullanan [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Bu sunucu şu anda kendi kendini barındırdığında varsayılan seçenek olan Katana.

> [!NOTE]
> Bu işlevsellik sunucular kullanılabilir olduğundan Katana şu anda OWIN ara yazılımı için Windows kimlik doğrulaması sağlamaz.


## <a name="windows-authentication-in-iis"></a>IIS Windows kimlik doğrulaması

Microsoft.Owin.Host.SystemWeb kullanarak, yalnızca IIS Windows kimlik doğrulamasını etkinleştirebilirsiniz.

"ASP.NET boş Web uygulaması" proje şablonunu kullanarak yeni bir ASP.NET uygulaması oluşturarak başlayalım.

![](enabling-windows-authentication-in-katana/_static/image1.png)

Ardından, NuGet paketleri ekleyin. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Şimdi adlı bir sınıf ekleyin `Startup` aşağıdaki kod ile:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Tüm IIS üzerinde çalışan OWIN için bir "Hello world" uygulaması oluşturmak için gereken budur. Uygulamada hata ayıklama için F5 tuşuna basın. "Hello World!" görmeniz gerekir tarayıcı penceresinde.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Ardından, IIS Express Windows kimlik doğrulamasını etkinleştirme. Gelen **Görünüm** menüsünde, select **özellikleri**. Proje özelliklerini görüntülemek için Çözüm Gezgini'nde proje adına tıklayın.

İçinde **özellikleri** penceresindeki ayarlayın **anonim kimlik doğrulaması** için **devre dışı** ve **Windows kimlik doğrulaması** için  **Etkin**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Visual Studio'dan uygulamayı çalıştırdığınızda, IIS Express kullanıcının Windows kimlik bilgileri gerektirir. Bu kullanarak bkz [Fiddler](http://fiddler2.com/home) veya başka bir HTTP hata ayıklama aracı. HTTP yanıtı örnek aşağıda verilmiştir:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Bu yanıt WWW-Authenticate üstbilgi sunucu desteklediğini belirtmek [anlaş](http://www.ietf.org/rfc/rfc4559.txt) bir protokol olan Kerberos veya NTLM kullanır.

Daha sonra bir sunucuya uygulamayı dağıttığınızda, izleyin [adımları](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) bu sunucuda IIS Windows kimlik doğrulamasını etkinleştirmek için.

## <a name="windows-authentication-in-httplistener"></a>HttpListener Windows kimlik doğrulaması

Katana kendi kendine barındırmak için Microsoft.Owin.Host.HttpListener kullanıyorsanız, doğrudan Windows kimlik doğrulamasını etkinleştirebilirsiniz **HttpListener** örneği.

İlk olarak, yeni bir konsol uygulaması oluşturun. Ardından, NuGet paketleri ekleyin. Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Şimdi adlı bir sınıf ekleyin `Startup` aşağıdaki kod ile:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Bu sınıf önce aynı "Hello world" örnek uygular, ancak aynı zamanda kimlik doğrulama düzeni olarak Windows kimlik doğrulaması ayarlar.

İçinde `Main` işlev, OWIN ardışık düzenini Başlat:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Uygulamayı Windows kimlik doğrulaması kullandığını doğrulayın Fiddler'da bir istek gönderebilir:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>İlgili Konular

[Project Katana’ya Genel Bakış](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[MVC 5 OWIN form kimlik doğrulamasını anlama](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
