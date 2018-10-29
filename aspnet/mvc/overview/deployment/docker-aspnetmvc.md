---
uid: mvc/overview/deployment/docker
title: ASP.NET MVC Uygulamalarını Windows Kapsayıcılarına Geçirme
description: Mevcut bir ASP.NET MVC uygulamasını alıp Windows Docker kapsayıcısında çalıştırma hakkında bilgi edinin
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: b3eb643daf230336ce5def96007b6096f86390e6
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148947"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>ASP.NET MVC Uygulamalarını Windows Kapsayıcılarına Geçirme

Bir Windows kapsayıcısında mevcut .NET Framework tabanlı uygulama çalıştırma uygulamanızı herhangi bir değişiklik gerektirmez. Bir Windows kapsayıcısında uygulamanızı çalıştırmak için uygulamanızı içeren bir Docker görüntüsü oluşturma ve kapsayıcı başlatın. Bu konu başlığında, mevcut bir gerçekleştirilecek açıklanmaktadır [ASP.NET MVC uygulaması](http://www.asp.net/mvc) ve bir Windows kapsayıcısında dağıtın.

Mevcut bir ASP.NET MVC uygulama ile başlayın ve ardından Visual Studio'yu kullanarak yayımlanan varlıkları oluşturun. Docker, içerir ve uygulamanızı çalıştıran görüntüyü oluşturmak için kullanın. Bir Windows kapsayıcısı içinde çalışan bir siteye göz atın ve uygulamanın çalıştığını doğrulayın.

Bu makalede, Docker'ın temel bir anlayış varsayılır. Okuyarak Docker hakkında bilgi edinebilirsiniz [Docker'a genel bakış](https://docs.docker.com/engine/understanding-docker/).

Bir kapsayıcıda çalıştıracaksınız rastgele sorular yanıtlanmaktadır basit bir Web uygulamasıdır. Bu uygulama, hiçbir kimlik doğrulaması veya veritabanı depolama alanı ile temel bir MVC uygulamasıdır; olanak tanır odaklanmak web katmanı için bir kapsayıcı devam ediliyor. Gelecekteki konuları taşımak ve kapsayıcılı uygulamaların kalıcı depolamayı yönetmek nasıl gösterir.

Uygulamanızı taşımak için aşağıdaki adımları içerir:

1. [Görüntü varlıkları oluşturmak için bir yayımlama görevi oluşturma.](#publish-script)
1. [Bir Docker görüntüsü oluşturma, uygulamanızı çalıştırın.](#build-the-image)
1. [Görüntünüzü çalıştıran bir Docker kapsayıcısı başlatılıyor.](#start-a-container)
1. [Tarayıcınızı kullanarak uygulamayı doğrulanıyor.](#verify-in-the-browser)

[Tamamlanmış uygulama](https://github.com/dotnet/samples/tree/master/framework/docker/MVCRandomAnswerGenerator) GitHub üzerinde bulunur.

## <a name="prerequisites"></a>Önkoşullar

Geliştirme makinesi aşağıdaki yazılımlar olmalıdır:

- [Windows 10 Yıldönümü güncelleştirmesi](https://www.microsoft.com/software-download/windows10/) (veya üzeri) veya [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (veya üzeri)
- [Windows için docker](https://docs.docker.com/docker-for-windows/) -sürüm kararlı 1.13.0 veya 1.12 Beta 26 (veya daha yeni sürümleri)
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

> [!IMPORTANT]
> Windows Server 2016'yı kullanıyorsanız, yönergelerini izleyin [kapsayıcı konağı dağıtma - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).

Docker'ı yükleyip sağ tıklatın ve Tepsi simgesi sonra **Windows kapsayıcılarına geç**. Bu, üzerinde Windows temelinde Docker görüntülerini çalıştırmak için gereklidir. Bu komutu yürütmek için birkaç saniye sürer:

![Windows kapsayıcı][windows-container]

## <a name="publish-script"></a>Betik yayımlama

Tek bir yerde bir Docker görüntüsüne yüklemeniz tüm varlıkları toplayın. Visual Studio kullanabileceğiniz **Yayımla** uygulamanız için bir yayımlama profili oluşturmak için komutu. Bu profili, bu öğreticide daha sonra hedef görüntünüzü kopyaladığınız bir dizin ağacındaki tüm varlıkları koyacaktır.

**Adımlar yayımlama**

1. Visual Studio'da web projesine sağ tıklayın ve seçin **Yayımla**.
1. Tıklayın **özel profil düğmesi**ve ardından **dosya sistemi** yöntemi olarak.
1. Dizin seçin. Kural gereği, indirilen örneğe kullanan `bin\Release\PublishOutput`.

![Bağlantı yayımlama][publish-connection]

Açık **dosya yayımlama seçeneği** bölümünü **ayarları** sekmesi. Seçin **yayımlama sırasında ön derleme**. Bu iyileştirme Docker kapsayıcısı görünümlerde derleme, önceden derlenmiş görünümleri Kopyalamakta olduğunuz anlamına gelir.

![Yayımlama ayarları][publish-settings]

Tıklayın **Yayımla**, ve Visual Studio gerekli tüm varlıkları hedef klasöre kopyalar.

## <a name="build-the-image"></a>Görüntüyü oluşturma

Docker görüntünüzü bir Dockerfile içinde tanımlayın. Dockerfile, temel görüntü, ek bileşenler, çalıştırmak istediğiniz uygulamayı ve diğer yapılandırma görüntüleri için yönergeler içerir.  Dockerfile giriştir `docker build` oluşturan görüntü komutu.

Temel alan bir görüntü oluşturacaksınız `microsoft/aspnet` görüntü bulunan [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).
Temel görüntü `microsoft/aspnet`, bir Windows Server görüntüsüdür. Bu, Windows Server Core, IIS ve ASP.NET 4.6.2 içerir. Bu görüntünün kapsayıcınızda çalıştırdığınızda, IIS otomatik olarak başlatılır ve yüklü Web sitelerini.

Dockerfile, görüntüyü oluşturan şöyle görünür:

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

Var olan hiçbir `ENTRYPOINT` bu Dockerfile içinde komutu. Gerekli değildir. Windows Server, IIS ile çalıştırırken, IIS aspnet temel görüntüde başlamak üzere yapılandırılmış giriş noktası işlemidir.

ASP.NET uygulamanızı çalıştıran görüntüyü oluşturmak için Docker derleme komutu çalıştırın. Bunu yapmak için proje dizininde bir PowerShell penceresi açın ve çözüm dizininde değil aşağıdaki komutu yazın:

```console
docker build -t mvcrandomanswers .
```

Bu komut Dockerfile içindeki yönergeleri kullanarak yeni görüntüyü oluşturur adlandırma (-t etiketi) görüntüyü mvcrandomanswers olarak. Bu temel görüntüden çekme içerebilir [Docker Hub](http://hub.docker.com)ve ardından uygulamanızı bu görüntü için ekleme.

Komut tamamlandıktan sonra çalıştırabileceğiniz `docker images` komutu yeni görüntü üzerindeki bilgileri görmek için:

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

Resim kimliği makinenize farklı olabilir. Şimdi uygulamayı çalıştıralım.

## <a name="start-a-container"></a>Bir kapsayıcı başlatma

Aşağıdakini yürüterek bir kapsayıcı başlatma `docker run` komutu:

```console
docker run -d --name randomanswers mvcrandomanswers
```

`-d` Docker görüntü ayrılmış modunda başlatmak için bağımsız değişken bildirir. Docker görüntüsünü geçerli kabuğundan bağlantısı kesilmiş çalıştıran anlamına gelir.

Çoğu docker örneklerde - kapsayıcı ve konak bağlantı noktalarını eşleme p görebilirsiniz. Varsayılan ASP.NET görüntü zaten kapsayıcı 80 numaralı bağlantı noktasında dinleyecek ve bunu kullanıma şekilde yapılandırdı.

`--name randomanswers` Çalışan kapsayıcıya bir ad verir. Bu ad, komutların çoğu kapsayıcı kimliği yerine kullanabilirsiniz.

`mvcrandomanswers` Başlatmak için görüntüsünü adıdır.

## <a name="verify-in-the-browser"></a>Tarayıcıda doğrulayın

> [!NOTE]
> Geçerli Windows kapsayıcı sürümü için göz atamazsınız `http://localhost`.
> Bu bilinen bir WinNAT, davranıştır ve gelecekte çözümlenir. Çözümlenene kadar kapsayıcının IP adresini kullanmanız gerekir.

Kapsayıcı başladıktan sonra çalışan kapsayıcınıza bir tarayıcıdan bağlanabilmek için IP adresini bulun:

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

IPv4 adresi, çalışan kapsayıcıya bağlanmak `http://172.31.194.61` ' de gösterilen örnekteki. Bu URL'yi tarayıcınıza yazın ve çalışan site görmeniz gerekir.

> [!NOTE]
> Bazı VPN veya proxy yazılım sitenize gitmesini engelliyor olabilir.
> Geçici olarak kapsayıcınızı çalıştığından emin olmak için devre dışı bırakabilirsiniz.

GitHub üzerinde örnek dizin içeren bir [PowerShell Betiği](https://github.com/dotnet/samples/blob/master/framework/docker/MVCRandomAnswerGenerator/run.ps1) , sizin için bu komutları yürütür. Bir PowerShell penceresi açın, dizin değiştirmek, çözüm dizini ve türü için:

```console
./run.ps1
```

Yukarıdaki komut görüntüyü oluşturur, makinenizde görüntülerin listesini görüntüler, bir kapsayıcısı başlatır ve bu kapsayıcı için IP adresini görüntüler.

Kapsayıcınızı durdurmak için sorun bir `docker
stop` komutu:

```console
docker stop randomanswers
```

Kapsayıcı kaldırmak için sorun bir `docker rm` komutu:

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Windows kapsayıcı için geçiş"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Dosya sistemi yayımlama"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Yayımlama ayarları"
