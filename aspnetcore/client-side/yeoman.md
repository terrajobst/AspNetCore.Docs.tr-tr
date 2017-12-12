---
title: "ASP.NET Core Yeoman ile proje oluşturma"
author: spboyer
description: "Bu makalede bir ASP.NET Core Yeoman kullanarak web uygulaması oluşturma sürecinde anlatılmaktadır macOS üzerinde Oluşturucu."
keywords: "ASP.NET Core, Yeoman, Çapraz Platform yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>ASP.NET Core Yeoman ile projeler derleme giriş

[Yeoman](http://yeoman.io/) birçok türdeki uygulamalar oluşturmak için bir proje yapı iskelesi sistemidir. Yeoman için ASP.NET Core oluşturucuyu yeni web, MVC veya konsol uygulaması başlatmak için proje şablonları çeşitli içerir.

## <a name="install-nodejs-npm-and-yeoman"></a>Node.js, npm ve Yeoman yükleyin

### <a name="prerequisites"></a>Önkoşullar

Node.js ve npm Yeoman için gereklidir. İndirin [Node.js](https://nodejs.org/). Yükleyici içerir [Node.js](https://nodejs.org/) ve [npm](https://www.npmjs.com/). Bower önyükleme gibi UI çerçeveleri yüklemek için gereklidir.

Yeoman ve Bower yüklemek için aşağıdaki komutu çalıştırın:

```console
npm install -g yo bower
```

>[!Note]
>Hata alırsanız `npm ERR! Please try running this command again as root/Administrator.` macOS üzerinde aşağıdaki komutu kullanarak çalıştırmak [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

Bir komut isteminden ASP.NET Oluşturucu yükleyin:

```console
npm install -g generator-aspnet
```

> [!NOTE]
> İzin hatası alırsanız, altında komutu çalıştırın `sudo` yukarıda açıklandığı gibi.

`–g` Böylece herhangi bir yol kullanılabilir bayrak oluşturucunun genel olarak yükler.

## <a name="create-an-aspnet-app"></a>Bir ASP.NET uygulaması oluşturma

Yeoman tabanlı ASP.NET Oluşturucu çalıştırın:

```console
yo aspnet
```

Oluşturucu bir menüsünü görüntüler. Aşağı ok **Web uygulaması temel [olmadan, üyelik ve yetkilendirme]** proje ve dokunun **Enter**:

![Komut penceresi: ne tür bir uygulama oluşturmak istiyor musunuz? Uygulama türleri menüsü](yeoman/_static/yeoman-yo-aspnet.png)

Önyükleme UI çerçevesi seçin ve dokunun **Enter**.

Kullan "**mywebapp şeklindedir**" uygulama adı ve ardından dokunun **Enter**.

Yeoman proje ve Tamamlayıcı dosyaları iskelesi. Önerilen sonraki adımlar da komutları biçiminde sağlanır.

![Komut penceresi: ASP.NET uygulamanızın adı nedir? Komut istemi](yeoman/_static/yeoman-yo-aspnet-created.png)

[ASP.NET Oluşturucu](https://www.npmjs.com/package/generator-aspnet) Visual Studio, Visual Studio Code yüklenmesi veya komut satırından çalıştırma projeler ASP.NET Core oluşturur.

## <a name="restore-build-and-run"></a>Geri yükleme, derleme ve çalıştırma

Önerilen komutları dizinleri değiştirerek izleyin `MyWebApp` dizin. Ardından çalıştırın `dotnet restore`.

![Komut penceresi](yeoman/_static/dotnet-restore.png)

Derleme ve Çalıştırma uygulamasını kullanarak `dotnet build` ve `dotnet run`:

![Komut penceresi](yeoman/_static/dotnet-build-run.png)

Bu noktada, yeni oluşturulan ASP.NET Core uygulama test etmek için gösterilen URL'yi gidebilirsiniz.

## <a name="client-side-packages"></a>İstemci tarafı paketleri

Ön uç kaynakları Yeoman şablonlar tarafından sağlanan oluşturucusunu kullanarak [Bower](xref:client-side/bower) ekleme, istemci-tarafı paket Yöneticisi *bower.json* ve *.bowerrc* Bower kullanarak istemci tarafı paketleri geri yüklenecek dosyaları.

[BundlerMinifier](xref:client-side/bundling-and-minification) bileşeni (paketleme) birleştirme kolaylığı ve CSS, JavaScript ve HTML küçültme için varsayılan olarak da bulunur.

## <a name="building-and-running-from-visual-studio"></a>Oluşturma ve Visual Studio'dan çalıştırma

Oluşturulan ASP.NET Core web projeniz Visual Studio'ya doğrudan yük sonra oluşturun ve projenizin oradan çalıştırın. Yeoman kullanarak yeni bir ASP.NET Core uygulaması iskele için yukarıdaki yönergeleri izleyin. Bu saati seçin **Web uygulaması** ad uygulaması ve menüden `MyWebApp`.

Visual Studio'yu açın. Dosya menüsünde açık ‣ proje/çözüm seçin.

Proje Aç iletişim kutusunda gidin *.csproj* dosyasını seçin ve tıklatın **açık** düğmesi. Çözüm Gezgini'nde proje aşağıdaki ekran görüntüsüne benzer görünmelidir.

![Dosya ve klasörleri Çözüm Gezgini'nde yeni proje](yeoman/_static/yeoman-solution.png)

Bir MVC web uygulaması yeoman scaffolds, hem tam sunucu ve istemci tarafı yapı desteği. Sunucu tarafı bağımlılıkları altında listelenen **bağımlılıkları/NuGet** düğümü ve istemci tarafı bağımlılıkları **bağımlılıkları/Bower** Çözüm Gezgini düğümünün. Proje yüklendiğinde bağımlılıkları otomatik olarak geri yüklenir.

![Çözüm Gezgini ağaç görünümünde bağımlılıklar düğümü altında Bower klasörü bağımlılıklarını listeleme açıktır.](yeoman/_static/yeoman-loading-dependencies.png)

Tüm bağımlılıkları geri yüklendiğinde basın **F5** projeyi çalıştırın. Varsayılan giriş sayfasını tarayıcıda görüntüler.

![Web uygulaması Microsoft Edge'de Aç](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>Geri yükleme, oluşturma ve bir komut satırından barındırma

Hazırlama ve .NET Core CLI kullanarak web uygulamanızı barındırmak.

Bir komut isteminde, geçerli dizin projeyi içeren klasöre geçin (diğer bir deyişle, klasör içeren *.csproj* dosyası):

```console
cd src\MyWebApp
```

Projenin NuGet Paket bağımlılıklarını geri yükleyin:

```console
dotnet restore
```

Uygulamayı çalıştırın:

```console
dotnet run
```

Platformlar arası [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu 5000 numaralı bağlantı noktasını dinlemeye başlayacak.

Bir web tarayıcısı açın ve gidin `http://localhost:5000`.

![Web uygulaması Microsoft Edge'de Aç](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Sub Oluşturucuları ile projenize ekleme

Yeoman kullanarak [sub oluşturucuları](https://github.com/omnisharp/generator-aspnet), ya da ekleyebilirsiniz bir `nuget.config` veya `web.config` Proje oluşturulduktan sonra. Örneğin, dosya oluşturulmalı dizininden aşağıdaki komutu yürütün:

```console
yo aspnet:nugetconfig
```

Adlı bir NuGet yapılandırma dosyası sonucudur `nuget.config` aşağıdaki içeriğe sahip:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Sunucuları (Kestrel ve WebListener)](xref:fundamentals/servers/index)
* [Temelleri](xref:fundamentals/index)
