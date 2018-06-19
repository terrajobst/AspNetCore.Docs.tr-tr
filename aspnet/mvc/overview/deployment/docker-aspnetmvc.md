---
uid: mvc/overview/deployment/docker
title: ASP.NET MVC Uygulamalarını Windows Kapsayıcılarına Geçirme
description: Var olan bir ASP.NET MVC uygulamasını alın ve bir Windows Docker kapsayıcısında çalıştırma hakkında bilgi edinin
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7a580c6c6236b375ea54ef4e9978fff6993d885a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
ms.locfileid: "29143195"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>ASP.NET MVC Uygulamalarını Windows Kapsayıcılarına Geçirme

Varolan bir .NET Framework tabanlı uygulamayı bir Windows kapsayıcıda çalışan uygulamanızı herhangi bir değişiklik gerektirmez. Bir Windows kapsayıcısında uygulamanızı çalıştırmak için uygulamayı içeren bir Docker görüntüsü oluşturun ve kapsayıcı başlatın. Bu konuda, var olan yapılacak açıklanmaktadır [ASP.NET MVC uygulaması](http://www.asp.net/mvc) ve bir Windows kapsayıcısında dağıtın.

Mevcut bir ASP.NET MVC uygulamasıyla başlayın, ardından Visual Studio kullanarak yayımlanan varlıklar oluşturun. Docker içeren ve uygulamanızın çalıştırdığı görüntüsü oluşturmak için kullanın. Bir Windows kapsayıcıda çalışan site bulun ve uygulamanın çalıştığını doğrulayın.

Bu makale, Docker temel bir anlayış varsayar. Okuyarak Docker hakkında bilgi edinebilirsiniz [Docker genel bakış](https://docs.docker.com/engine/understanding-docker/).

Bir kapsayıcıda çalıştıracaksınız uygulama rastgele sorular yanıtlanmaktadır basit bir Web sitesidir. Bu uygulama, hiçbir kimlik doğrulama veya veritabanı depolama ile temel bir MVC uygulamasıdır; sağlar odaklanmak web katmanı için bir kapsayıcı devam ediliyor. Gelecekteki konuları taşıyın ve kalıcı depolama kapsayıcılı uygulamalarında yönetmek nasıl yapacağınızı gösterir.

Uygulamanızı taşıma adımları içerir:

1. [Varlıkları için bir görüntü oluşturmak için bir yayımlama görevi oluşturma.](#publish-script)
1. [Docker görüntü oluşturmaya uygulamanızı çalıştırın.](#build-the-image)
1. [Görüntünüzü çalıştıran bir Docker kapsayıcısı başlatılıyor.](#start-a-container)
1. [Tarayıcınızı kullanarak uygulama doğrulanıyor.](#verify-in-the-browser)

[Tamamlandı uygulama](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) GitHub üzerinde değil.

## <a name="prerequisites"></a>Önkoşullar

Geliştirme makinenin çalışıyor olması gerekir

- [Windows 10 Anniversary güncelleştirme](https://www.microsoft.com/software-download/windows10/) (veya üstü) veya [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (veya üstü).
- [Windows için docker](https://docs.docker.com/docker-for-windows/) -sürüm kararlı 1.13.0 veya 1.12 Beta 26 (veya daha yeni sürümleri)
- [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx).

> [!IMPORTANT]
> Windows Server 2016 kullanıyorsanız, yönergeleri izleyin [kapsayıcı konak dağıtımı - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Yükleme ve Docker, başlangıç sağ sonra seçin ve Tepsi simgesi üzerinde **geçiş Windows kapsayıcılara**. Bu, Windows temelinde Docker görüntüleri çalıştırmak için gereklidir. Bu komutu yürütmek için birkaç saniye sürer:

![Windows kapsayıcısı][windows-container]

## <a name="publish-script"></a>Komut dosyası yayımlama

Tek bir yerde Docker görüntüye yüklemek için gereken tüm varlıklarını toplayın. Visual Studio'yu kullanabilirsiniz **Yayımla** uygulamanız için bir yayımlama profili oluşturmak için komutu. Bu profil, daha sonra Bu öğreticide, hedef görüntüyü kopyalamak bir dizin ağacındaki tüm varlıklarını sokar.

**Adımları yayımlama**

1. Visual Studio'da web projesine sağ tıklayın ve seçin **Yayımla**.
1. Tıklatın **özel profil düğmesini**ve ardından **dosya sistemi** yöntemi olarak.
1. Dizini seçin. Kurala göre indirilen örnek kullanır `bin\Release\PublishOutput`.

![Bağlantı yayımlama][publish-connection]

Açık **dosya yayımlama seçeneği** bölümünü **ayarları** sekmesi. Seçin **yayımlama sırasında ön derleme**. Bu iyileştirme Docker kapsayıcısı görünümlerde derleme, önceden derlenmiş görünümleri Kopyalamakta olduğunuz anlamına gelir.

![Yayımlama ayarları][publish-settings]

Tıklatın **Yayımla**, ve Visual Studio gerekli tüm varlıklarını hedef klasöre kopyalar.

## <a name="build-the-image"></a>Görüntü oluşturma

Docker görüntünüzü bir Dockerfile tanımlayın. Dockerfile temel görüntü, ek bileşenleri, çalıştırmak istediğiniz uygulama ve diğer yapılandırma görüntüleri için yönergeler içerir.  Dockerfile girdidir `docker build` görüntüsünü oluşturur komutu.

Bir görüntüyü dayalı oluşturacaksınız `microsoft/aspnet` görüntü bulunan [Docker hub'a](https://hub.docker.com/r/microsoft/aspnet/).
Temel görüntü `microsoft/aspnet`, bir Windows Server görüntüdür. Windows Server Core, IIS ve ASP.NET 4.6.2 içerir. Bu görüntü, kapsayıcısında çalıştırdığınızda, IIS otomatik olarak başlar ve Web siteleri yüklü.

Görüntünüzü oluşturur Dockerfile şöyle görünür:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Var olan hiçbir `ENTRYPOINT` bu Dockerfile komutu. Bir gerekmez. Windows Server IIS ile çalışırken, IIS aspnet temel görüntü başlatılacak şekilde yapılandırılmış entrypoint işlemidir.

ASP.NET uygulamanızın çalıştırdığı görüntüsü oluşturmak için Docker yapı komutunu çalıştırın. Bunu yapmak için projenizin dizininde bir PowerShell penceresi açın ve çözüm dizinde aşağıdaki komutu yazın:

```console
docker build -t mvcrandomanswers .
```

Bu komut, Dockerfile yönergeleri kullanarak yeni görüntüsü oluşturacak adlandırma (-t etiketleme) görüntüyü mvcrandomanswers olarak. Bu ana görüntüden çekme içerebilir [Docker hub'a](http://hub.docker.com)ve ardından uygulamanızı, görüntü ekleme.

Komut tamamlandığında çalıştırabilirsiniz `docker images` üzerinde yeni bir görüntü bilgileri görmek için komutu:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Resim kimliği makinenizde farklı olacaktır. Şimdi, şimdi uygulamayı çalıştırın.

## <a name="start-a-container"></a>Bir kapsayıcı Başlat

Bir kapsayıcı aşağıdakini yürüterek Başlat `docker run` komutu:

```console
docker run -d --name randomanswers mvcrandomanswers
```

`-d` Bağımsız değişkeni görüntü ayrılmış modunda başlatmak için Docker söyler. Docker görüntü geçerli Kabuğu'ndan bağlantısı kesilmiş çalıştıran anlamına gelir.

Birçok docker örneklerde - kapsayıcı ve ana bilgisayar bağlantı noktalarını eşleştirmek için p görebilirsiniz. Varsayılan aspnet görüntüsü zaten bağlantı noktası 80 üzerinde dinler ve onu kullanıma sunmak için kapsayıcıyı yapılandırdı. 

`--name randomanswers` Çalışan kapsayıcı için bir ad sağlar. Çoğu komutlarda kapsayıcı kimliği yerine bu adı kullanın.

`mvcrandomanswers` Başlatmak için görüntüsünü adıdır.

## <a name="verify-in-the-browser"></a>Tarayıcıda doğrulayın

> [!NOTE]
> Geçerli Windows kapsayıcı sürümüyle için göz atamazsınız `http://localhost`.
> Bu bir bilinen bir WinNAT davranıştır ve gelecekte çözümlenir. Çözümlenene kadar kapsayıcı IP adresini kullanmanız gerekir.

Kapsayıcı başladıktan sonra çalışan bir kapsayıcıya bir tarayıcıdan bağlanabilmesi için IP adresini bulun:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

IPv4 adresini kullanarak çalışan kapsayıcıya bağlanmak `http://172.31.194.61` gösterilen örnekte. Bu URL'yi tarayıcınıza yazın ve çalışan site görmeniz gerekir.

> [!NOTE]
> Bazı VPN veya ara yazılım sitenize gezinme engelleyebilir.
> Geçici olarak, kapsayıcı çalıştığından emin olmak için devre dışı bırakabilirsiniz.

Github'da örnek dizini içeren bir [PowerShell Betiği](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) , sizin için bu komutları yürütür. Bir PowerShell penceresi açın, dizini, çözüm dizini ve türü için değiştirin:

```console
./run.ps1
```

Yukarıdaki komut görüntü oluşturur, makinenizde görüntüleri listesini görüntüler, bir kapsayıcı başlatır ve bu kapsayıcı için IP adresini görüntüler.

Kapsayıcı durdurmak için sorunu bir `docker
stop` komutu:

```console
docker stop randomanswers
```

Kapsayıcı kaldırmak için sorunu bir `docker rm` komutu:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Windows kapsayıcıya geçiş"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Dosya sistemi için yayımlayın"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Yayımlama ayarları"
