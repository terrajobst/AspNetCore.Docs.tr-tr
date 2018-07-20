---
title: ASP.NET Core ile Docker için Visual Studio Araçları
author: spboyer
description: Bir ASP.NET Core uygulamasını kapsayıcılı hale getirme için Visual Studio 2017 araçları ve Docker için Windows kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146890"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>ASP.NET Core ile Docker için Visual Studio Araçları

[Visual Studio 2017](https://www.visualstudio.com/) oluşturma, hata ayıklama ve kapsayıcılı ASP.NET Core .NET Core'u hedefleyen uygulamaların çalıştırılmasını destekler. Hem Windows hem de Linux kapsayıcıları desteklenmektedir.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/) ile **.NET Core çoklu platform geliştirme** iş yükü
* [Windows için docker](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Yükleme ve Kurulum

Docker yükleme için bölümündeki bilgileri gözden geçirin [için Docker Windows: yüklemeden önce bilinmesi gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [için Docker Windows](https://docs.docker.com/docker-for-windows/install/).

**[Sürücüleri paylaşılan](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker birimi eşlemenin ve hata ayıklamayı destekleyecek şekilde yapılandırılması gerekir. Sistem tepsisi'nın Docker simgesine sağ tıklayın, **ayarları...** seçip **paylaşılan sürücüleri**. Docker dosyaları depoladığı sürücü seçin. Seçin **uygulamak**.

![Paylaşılan sürücüleri](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 sürüm 15.6 ve daha sonra sor **paylaşılan sürücüleri** yapılandırılmamışlardır.

## <a name="add-docker-support-to-an-app"></a>Docker desteği için uygulama ekleme

Docker desteği eklemek için bir ASP.NET Core projesi için projenin .NET Core hedeflemesi gerekir. Hem Linux hem de Windows kapsayıcıları desteklenmektedir.

Bir projeye Docker desteği eklenirken bir Windows veya Linux kapsayıcısı seçin. Docker konağı aynı kapsayıcı türü çalıştırmalıdır. Çalışan Docker örneğinde kapsayıcı türü değiştirmek için sistem tepsisindeki'nın Docker simgesini sağ tıklatın ve seçin **Windows kapsayıcılarına geç...**  veya **geçiş Linux kapsayıcıları için...** .

### <a name="new-app"></a>Yeni uygulama

İle yeni bir uygulama oluştururken **ASP.NET Core Web uygulaması** proje şablonları, select **Docker desteğini etkinleştir** onay kutusunu:

![Docker desteği onay kutusunu etkinleştirin](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

Hedef çerçeveyi .NET Core ise **işletim sistemi** açılan bir kapsayıcı türünün seçimini sağlar.

### <a name="existing-app"></a>Mevcut uygulama

Docker için Visual Studio Araçları, .NET Framework'ü hedefleyen var olan bir ASP.NET Core projesine Docker ekleme desteklemez. .NET Core'u hedefleyen ASP.NET Core projeleri için araç kullanımı aracılığıyla Docker desteği eklemek için iki seçenek vardır. Projeyi Visual Studio'da açın ve aşağıdaki seçeneklerden birini belirleyin:

* Seçin **Docker desteği** gelen **proje** menüsü.
* Çözüm Gezgini'nde projeye sağ tıklayıp **Ekle** > **Docker desteği**.

## <a name="docker-assets-overview"></a>Docker varlıkları genel bakış

Docker için Visual Studio Araçları ekleme bir *docker-compose* aşağıdaki dosyaları içeren çözüme:

* *.dockerignore*: bir derleme bağlamı oluşturulurken dışlanacak dosya ve dizin desenlerinin bir listesi içerir.
* *docker-compose.yml*: temel [Docker Compose](https://docs.docker.com/compose/overview/) oluşturulabilen ve çalıştırmak için görüntü koleksiyonunu tanımlamak için kullanılan dosya `docker-compose build` ve `docker-compose run`sırasıyla.
* *docker-compose.override.yml*: isteğe bağlı bir dosya okuma Docker Compose tarafından yapılandırmasını içeren hizmetler için geçersiz kılar. Visual Studio yürütür `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` bu dosyaları birleştirmek için.

A *Dockerfile*, son bir Docker görüntüsü oluşturmak için tarif, proje kök dizinine eklenir. Başvurmak [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) içindeki komutları anlaşılması için. Bu özellikle *Dockerfile* kullanan bir [çok aşamalı derleme](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) adlı ayrı, dört içeren derleme aşamaları:

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Dockerfile* dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü. Bu temel görüntü başlangıç performansını artırmak için pre-jıtted olan ASP.NET Core NuGet paketleri içerir.

*Docker-compose.yml* dosyası Proje çalıştırıldığında, oluşturulan görüntünün adını içerir:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

Önceki örnekte `image: hellodockertools` görüntü oluşturur `hellodockertools:dev` uygulamayı çalıştırdığında **hata ayıklama** modu. `hellodockertools:latest` Görüntü uygulama çalıştırıldığında oluşturulan **yayın** modu.

Görüntü adı ile önek [Docker Hub](https://hub.docker.com/) kullanıcı adı (örneğin, `dockerhubusername/hellodockertools`), görüntünün kayıt defterine gönderilir. Alternatif olarak, özel kayıt defteri URL'si eklemek için görüntü adı değiştirin (örneğin, `privateregistry.domain.com/hellodockertools`) yapılandırmasına bağlı olarak.

## <a name="debug"></a>Hata ayıklama

Seçin **Docker** gelen hata ayıklama açılır araç ve uygulama hata ayıklamayı başlatın. **Docker** görünümünü **çıkış** penceresi, aşağıdaki eylemler alma yeri gösterir:

* *Microsoft/aspnetcore* çalışma zamanı görüntü alındı (henüz önbellekte değilse).
* *Microsoft/aspnetcore-build* görüntüsü derleme ve yayınlama alınan (henüz önbellekte değilse).
* *ASPNETCORE_ENVIRONMENT* ortam değişkeni ayarlandığında `Development` kapsayıcı içindeki.
* 80 numaralı bağlantı noktasını kullanıma sunulan ve localhost için dinamik olarak atanan bağlantı noktasına eşlenen. Bağlantı noktası Docker konak tarafından belirlenir ve ile sorgulanabilir `docker ps` komutu.
* Uygulama, kapsayıcıya kopyalanır.
* Varsayılan tarayıcı hata ayıklayıcısı ekli dinamik olarak atanan bağlantı noktasını kullanarak kapsayıcıya ile başlatılır.

Sonuçta elde edilen Docker görüntü *geliştirme* uygulamayla görüntüsü *microsoft/aspnetcore* temel görüntü olarak görüntüler. Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi. Makine görüntülerinde görüntülenir:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Geliştirme görüntü olarak uygulama içerikleri eksik **hata ayıklama** yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullanın. Görüntü gönderebilmeniz için kullanmak **yayın** yapılandırma.

Çalıştırma `docker ps` PMC komutunu. Uygulamayı kullanarak kapsayıcı çalıştırma dikkat edin:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Düzenle ve devam et

Statik dosyalar ve Razor görünümleri değişiklikler, bir derleme adımı gerek kalmadan otomatik olarak güncelleştirilir. Kaydet ve güncelleştirme görmek için tarayıcıyı yenileyin değişikliği yapın.

Derleme ve yeniden başlatılmasını Kestrel kapsayıcı içindeki kod dosya değişiklikleri gerektirir. Değişikliği yaptıktan sonra kullanın `CTRL+F5` işlemini gerçekleştirmek ve uygulama kapsayıcı içinde başlatın. Docker kapsayıcısı yeniden veya durduruldu. Çalıştırma `docker ps` PMC komutunu. Uyarı: orijinal kapsayıcıdan itibarıyla 10 dakika önce hala çalışıyor

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Docker görüntülerini yayımlama

Uygulama geliştirme ve hata ayıklama döngüsünü tamamlandıktan sonra Docker için Visual Studio Araçları, uygulamayı üretim görüntüsü oluşturmada yardımcı olacak. Aşağı açılan yapılandırmasını değiştirmek **yayın** ve bir uygulama geliştirin. Araç görüntüyle üretir *son* özel kayıt defteri ya da Docker hub'dan gönderilen etiketi.

Çalıştırma `docker images` komutunu PMC'yi görüntülerin listesini görüntülemek için:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` Depo adları ile Ara görüntü komutu döndürür ve etiketleri tanımlanan olarak  *\<yok >* (Yukarıda listelenmeyen). Bu görüntüleri adlandırılmamış tarafından üretilen [çok aşamalı derleme](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*. Bunlar, son görüntü oluşturma verimliliğini artırmak&mdash;değişiklikler olduğunda yalnızca gerekli Katmanlar yeniden oluşturulur. Aracı görüntüleri artık gerekli değilse, bunları silin kullanarak [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.

Bir karşılaştırma için boyut olarak daha küçük üretim ya da sürüm resmi beklentisi olabilir *geliştirme* görüntü. Birimi eşlemenin nedeniyle, yerel makine ve kapsayıcı içinde değil hata ayıklayıcı ve uygulamanın çalışıyordu. *Son* görüntü, bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli uygulama kodu paketlenmiş. Bu nedenle, delta uygulama kodu boyutudur.

## <a name="additional-resources"></a>Ek kaynaklar

* [Visual Studio 2017 geliştirme Docker ile ilgili sorunları giderme](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Docker GitHub deposu için Visual Studio Araçları](https://github.com/Microsoft/DockerTools)
