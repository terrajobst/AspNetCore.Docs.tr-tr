---
title: ASP.NET Core için docker görüntüleri
author: tdykstra
description: Docker kayıt defterinden yayımlanan .NET Core Docker görüntüleri kullanmayı öğrenin. Görüntüleri çekmek ve kendi görüntülerinizi oluşturun.
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/docker/building-net-docker-images
ms.openlocfilehash: ea96ae6d36c7e8320ea49e666a807ece72645865
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207790"
---
# <a name="docker-images-for-aspnet-core"></a>ASP.NET Core için docker görüntüleri

Bu öğreticide, ASP.NET Core uygulaması Docker kapsayıcılarında çalıştırmak gösterilir.

Bu öğreticide şunları yaptınız:
> [!div class="checklist"]
> * Microsoft .NET Core Docker görüntüleri hakkında bilgi edinin 
> * Bir ASP.NET Core örnek uygulamasını indirin
> * Örnek uygulamayı yerel olarak çalıştırma
> * Örnek uygulamayı Linux kapsayıcıları çalıştırma
> * Windows kapsayıcıları örnek uygulamayı çalıştırma
> * Derleme ve el ile dağıtma

## <a name="aspnet-core-docker-images"></a>ASP.NET Core Docker görüntüleri

Bu öğreticide bir ASP.NET Core örnek uygulamasını indirin ve Docker kapsayıcılarında çalıştırın. Örnek, hem Linux hem de Windows kapsayıcıları ile çalışır.

Dockerfile'ı kullanan örnek [Docker çok aşamalı yapı özelliği](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) oluşturun ve farklı kapsayıcılarda çalıştırın. Derleme ve çalıştırma kapsayıcılar, Docker Hub'ında, Microsoft tarafından sağlanan görüntülerden oluşturulur:

* `dotnet/core/sdk`

  Örnek uygulama oluşturmak için bu görüntü kullanır. Görüntü (CLI) komut satırı araçları içerir .NET Core SDK'sı içerir. Görüntü, yerel geliştirme, hata ayıklama ve birim testi için optimize edilmiştir. Geliştirme ve derleme için yüklenen araçlar bu oldukça büyük bir görüntü yapın. 

* `dotnet/core/aspnet` 

   Örnek uygulamayı çalıştırmak için bu görüntü kullanır. Görüntü ASP.NET Core çalışma zamanı ve kitaplıkları içerir ve üretim uygulamaları çalıştırmak için optimize edilmiştir. Docker kayıt defterinden Docker konağı ağ performansı en iyi duruma getirilmiş hızlı dağıtım ve uygulama başlatma için tasarlanmış, görüntünün görece küçük olduğundan. Yalnızca ikili dosyaları ve bir uygulamayı çalıştırmak için gereken içeriği kapsayıcıya kopyalanır. İçeriği çalıştırılmaya hazır durumda en hızlı süreye etkinleştirme `Docker run` için uygulama başlatma. Dinamik kod derlemesi, Docker modelde gerek yoktur.

## <a name="prerequisites"></a>Önkoşullar

* [.NET core SDK'sını 2.2](https://www.microsoft.com/net/core)

* Docker istemcisi 18.03 veya üzeri

  * Linux dağıtımları
    * [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
    * [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
    * [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
    * [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
  * [macOS](https://docs.docker.com/docker-for-mac/install/)
  * [Windows](https://docs.docker.com/docker-for-windows/install/)

* [Git](https://git-scm.com/download)

## <a name="download-the-sample-app"></a>Örnek uygulamasını indirin

* Örnek kopyalayarak indirme [.NET Core Docker deposunda](https://github.com/dotnet/dotnet-docker): 

  ```console
  git clone https://github.com/dotnet/dotnet-docker
  ```

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

* Proje klasörüne gidin *dotnet-docker/samples/aspnetapp/aspnetapp*.

* Uygulamayı yerel olarak oluşturup çalıştırmak için aşağıdaki komutu çalıştırın:

  ```console
  dotnet run
  ```

* Git `http://localhost:5000` uygulamayı test etmek için bir tarayıcıda.

* CTRL + C tuşlarına basın uygulamayı durdurmak için komut isteminde.

## <a name="run-in-a-linux-container"></a>Bir Linux kapsayıcısında çalıştırma

* Linux kapsayıcıları için Docker istemcisinde geçin.

* Dockerfile klasörde gidin *dotnet-docker/samples/aspnetapp*.

* Derleme ve Docker örneği çalıştırmak için aşağıdaki komutları çalıştırın:

  ```console
  docker build -t aspnetapp .
  docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
  ```

  `build` Komut bağımsız değişkenleri:
  * Görüntü aspnetapp adı.
  * Dockerfile geçerli klasörde (dönem sonunda) arayın.

  Run komutu bağımsız değişkenleri:
  * Sahte TTY ayırma ve açık iliştirilmemiş bile saklayın. (Aynı efekt olarak `--interactive --tty`.)
  * Otomatik olarak çıktığında kapsayıcı kaldırın.
  * Bağlantı noktası 5000 yerel makinede kapsayıcı 80 numaralı bağlantı noktasına eşler.
  * Kapsayıcı aspnetcore_sample adı.
  * Aspnetapp görüntüsünü belirtin.

* Git `http://localhost:5000` uygulamayı test etmek için bir tarayıcıda.

## <a name="run-in-a-windows-container"></a>Bir Windows kapsayıcısında çalıştırma

* Windows kapsayıcıları için Docker istemcisinde geçin.

Docker dosya klasörde gidin `dotnet-docker/samples/aspnetapp`.

* Derleme ve Docker örneği çalıştırmak için aşağıdaki komutları çalıştırın:

  ```console
  docker build -t aspnetapp .
  docker run -it --rm --name aspnetcore_sample aspnetapp
  ```

* Windows kapsayıcıları için kapsayıcının IP adresi gerekir (göz `http://localhost:5000` çalışmaz):
  * Başka bir komut istemi açın.
  * Çalıştırma `docker ps` çalışan kapsayıcıları görmek için. "Aspnetcore_sample" kapsayıcı var olduğunu doğrulayın.
  * Çalıştırma `docker exec aspnetcore_sample ipconfig` kapsayıcının IP adresini görüntülemek için. Komut çıktısı şu örnekteki gibi görünür:

    ```console
    Ethernet adapter Ethernet:

       Connection-specific DNS Suffix  . : contoso.com
       Link-local IPv6 Address . . . . . : fe80::1967:6598:124:cfa3%4
       IPv4 Address. . . . . . . . . . . : 172.29.245.43
       Subnet Mask . . . . . . . . . . . : 255.255.240.0
       Default Gateway . . . . . . . . . : 172.29.240.1
    ```

* ' % S'kapsayıcı IPv4 adresi (örneğin, 172.29.245.43) kopyalayın ve uygulamayı test etmek için tarayıcınızın adres çubuğuna yapıştırın.

## <a name="build-and-deploy-manually"></a>Derleme ve el ile dağıtma

Bazı senaryolarda, uygulama için çalışma zamanında gereken uygulama dosyaları kopyalamak için bir kapsayıcı dağıtmak isteyebilirsiniz. Bu bölümde, el ile dağıtma işlemi gösterilmektedir.

* Proje klasörüne gidin *dotnet-docker/samples/aspnetapp/aspnetapp*.

* Çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu:

  ```console
  dotnet publish -c Release -o published
  ```

  Komut bağımsız değişkenleri:
  * Uygulama yayın modunda (hata ayıklama modu varsayılandır) oluşturun.
  * Dosyalar oluşturma *yayımlanan* klasör.

* Uygulamayı çalıştırın.

  * Windows:

    ```console
    dotnet published\aspnetapp.dll
    ```

  * Linux:

    ```bash
    dotnet published/aspnetapp.dll
    ```

* Gözat `http://localhost:5000` giriş sayfasını görmek için.

### <a name="the-dockerfile"></a>Dockerfile

Tarafından kullanılan Dockerfile burada'nın `docker build` daha önce çalıştırdığınız komutu.  Kullandığı `dotnet publish` derlemek ve dağıtmak için bu bölümde yaptığınız gibi.  

```console
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

# copy everything else and build app
COPY aspnetapp/. ./aspnetapp/
WORKDIR /app/aspnetapp
RUN dotnet publish -c Release -o out


FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS runtime
WORKDIR /app
COPY --from=build /app/aspnetapp/out ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Docker derleme komutu](https://docs.docker.com/engine/reference/commandline/build)
* [Docker run komutu](https://docs.docker.com/engine/reference/commandline/run)
* [ASP.NET Core Docker örnek](https://github.com/dotnet/dotnet-docker) (Bu öğreticide kullanılan bir.)
* [ASP.NET Core, proxy sunucuları ile çalışma ve yük Dengeleyiciler için yapılandırma](/aspnet/core/host-and-deploy/proxy-load-balancer)
* [Visual Studio Docker araçları ile çalışma](https://docs.microsoft.com/aspnet/core/publishing/visual-studio-tools-for-docker)
* [Visual Studio kodu ile hata ayıklama](https://code.visualstudio.com/docs/nodejs/debugging-recipes#_debug-nodejs-in-docker-containers) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:
> [!div class="checklist"]
> * Microsoft .NET Core Docker görüntüleri hakkında bilgi edindiniz 
> * Bir ASP.NET Core örnek uygulama indirilebilir
> * Örnek uygulamayı yerel olarak çalıştırma
> * Örnek uygulamayı Linux kapsayıcıları çalıştırma
> * Windows kapsayıcıları ile örneği çalıştırma
> * Yerleşik ve el ile dağıtılabilir

Örnek uygulamayı içeren bir Git deposu belgeleri de içerir. Depodaki kaynakları genel bakış için bkz. [Benioku dosyasını](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/README.md). Özellikle, HTTPS uygulamayı öğrenin:

> [!div class="nextstepaction"]
> [HTTPS üzerinden Docker ile ASP.NET Core uygulamaları geliştirme](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/aspnetcore-docker-https-development.md)
