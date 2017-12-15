---
title: "ASP.NET çekirdeği ile Docker için Visual Studio Araçları"
author: spboyer
description: "Visual Studio 2017 araçları ve Windows için Docker ASP.NET Core uygulama containerize için nasıl kullanılacağını öğrenin."
keywords: "Docker için ASP.NET Core, Docker, VS araçları"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 5bdd9317a69a1e9f1178d203595e25bc84881811
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>ASP.NET çekirdeği ile Docker için Visual Studio Araçları

[Visual Studio 2017](https://www.visualstudio.com/) oluşturma, hata ayıklama ve ASP.NET Core uygulamaları .NET Framework veya .NET Core hedefleme çalıştırma destekler. Hem Windows hem de Linux kapsayıcıları desteklenir.

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017](https://www.visualstudio.com/) ile **.NET Core platformlar arası geliştirme** iş yükü
- [Windows için docker](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Yükleme ve Kurulum

Docker yükleme için bilgileri gözden geçirmeniz [Windows için Docker: yüklemeden önce bilmeniz gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [Docker için Windows](https://docs.docker.com/docker-for-windows/install/).

**[Sürücüleri paylaşılan](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker birimi eşlemenin ve hata ayıklama desteklemek için yapılandırılması gerekir. Sistem tepsisi'nın Docker simgesine sağ tıklayın, **ayarları...** seçip **paylaşılan sürücüleri**. Docker dosyalarınızı depoladığı sürücüyü seçin ve'ı tıklatın **Uygula**.

![Paylaşılan sürücüler](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 15.6 ve sonraki sürümleri isteminde zaman **paylaşılan sürücüleri** yapılandırılmamışlardır.

## <a name="add-docker-support-to-an-app"></a>Docker desteği için bir uygulama ekleyin

ASP.NET Core projenin hedef çerçevesi desteklenen kapsayıcı türleri belirler. Proje .NET Core hedefleme hem Linux hem de Windows kapsayıcıları destekler. Proje yalnızca .NET Framework hedefleme Windows kapsayıcıları destekler.

Docker destek projenize eklerken, bir Windows ya da Linux kapsayıcısı seçin. Docker ana bilgisayar aynı kapsayıcı türü çalıştırması gerekir. Kapsayıcı türü çalışan Docker örneğinde değiştirmek için sistem tepsisi'nın Docker simgesini sağ tıklatın ve seçin **geçiş Windows kapsayıcılara...**  veya **geçiş Linux kapsayıcılara...** .

### <a name="new-app"></a>Yeni uygulama

Yeni bir uygulamayla oluştururken **ASP.NET çekirdek Web uygulaması** proje şablonları, select **Docker desteğini etkinleştir** onay kutusu:

![Docker destek onay kutusunu etkinleştir](./visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

Hedef çerçevesini .NET Core ise **OS** açılan için bir kapsayıcı türü seçimi sağlar.

### <a name="existing-app"></a>Mevcut uygulama

Docker için Visual Studio Araçları, .NET Framework'ü hedefleme mevcut bir ASP.NET Core projesine Docker ekleme desteklemez. .NET Core hedefleme ASP.NET Core projeleri için araç üzerinden Docker desteği eklemek için iki seçenek vardır. Visual Studio'da projeyi açın ve aşağıdaki seçeneklerden birini seçin:

- Seçin **Docker Destek** gelen **proje** menüsü.
- Çözüm Gezgini'nde projeye sağ tıklayıp **Ekle** > **Docker Destek**.

## <a name="docker-assets-overview"></a>Docker varlıklar genel bakış

Docker için Visual Studio Araçları Ekle bir *docker-oluşturan* proje çözüme aşağıdakileri içeren:
- *.dockerignore*: bir yapı bağlamı oluşturulurken dışlanacak dosya ve dizin desenlerinin bir listesi içerir.
- *docker-compose.yml*: temel [Docker Compose](https://docs.docker.com/compose/overview/) oluşturulur ve çalıştırmak için resimler koleksiyonunu tanımlamak için kullanılan dosya `docker-compose build` ve `docker-compose run`sırasıyla.
- *docker compose.override.yml*: isteğe bağlı bir dosya okuma Docker Compose tarafından yapılandırmasını içeren hizmetler için geçersiz kılar. Visual Studio yürütür `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` bu dosyaları birleştirmek için.

A *Dockerfile*, son Docker görüntü oluşturmak için tarif proje köküne eklenir. Başvurmak [Dockerfile başvuru](https://docs.docker.com/engine/reference/builder/) içindeki komutları anlaşılması için. Bu belirli *Dockerfile* kullanan bir [çok aşama yapı](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) adlı derleme aşamaları dört farklı içeren:

[!code-text[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Dockerfile* dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü. Bu temel görüntü başlangıç performansını artırmak için pre-jitted silinmiş ASP.NET Core NuGet paketlerini içerir.

*Docker-compose.yml* dosyası projeye çalıştığında oluşturduğunuz görüntü adı içerir:

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

Önceki örnekte `image: hellodockertools` görüntü oluşturur `hellodockertools:dev` uygulama çalıştırıldığında **hata ayıklama** modu. `hellodockertools:latest` Görüntü uygulama çalıştırıldığında oluşturulan **sürüm** modu.

Görüntü adı ile önek, [Docker hub'a](https://hub.docker.com/) kullanıcı adı (örneğin, `dockerhubusername/hellodockertools`) kayıt defterine Görüntü göndermeyi planlıyorsanız. Alternatif olarak, özel kayıt defteri URL'nizi içerecek şekilde görüntü adı değiştirin (örneğin, `privateregistry.domain.com/hellodockertools`) yapılandırmanıza bağlı olarak.

## <a name="debug"></a>Hata ayıklama

Seçin **Docker** gelen hata ayıklama açılır araç ve uygulama hata ayıklamayı Başlat. **Docker** görünümünü **çıkış** penceresi, aşağıdaki eylemler alma yeri gösterir:

- *Microsoft/aspnetcore* çalışma zamanı görüntü edinilen (henüz önbelleğindeki, varsa).
- *Aspnetcore/microsoft-yapı* derleme ve yayımlama görüntü edinilen (henüz önbelleğindeki, varsa).
- *ASPNETCORE_ENVIRONMENT* ortam değişkeni ayarlanır `Development` kapsayıcı içinde.
- Bağlantı noktası 80 kullanıma sunulan ve localhost için dinamik olarak atanan bir bağlantı noktası eşlenir. Bağlantı noktası Docker ana bilgisayarı tarafından belirlenir ve ile sorgulanabilir `docker ps` komutu.
- Uygulamanız için kapsayıcı kopyalanır.
- Varsayılan tarayıcı dinamik olarak atanan bağlantı noktasını kullanan kapsayıcıya hata ayıklayıcısı ekli başlatılır. 

Sonuçta elde edilen Docker görüntü *geliştirme* , uygulamanızın görüntüsü ile *microsoft/aspnetcore* temel görüntü olarak görüntüler. Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi. Makinenizde görüntüleri görüntülenir:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> Geliştirme görüntü olarak, uygulama içeriği eksik **hata ayıklama** yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullanın. Bir görüntü göndermek için kullanmanız **sürüm** yapılandırma.

Çalıştırma `docker ps` PMC komutu. Bir kapsayıcı kullanılarak uygulama çalışırken dikkat edin:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>Düzenle ve devam et

Statik dosyalar ve Razor görünümleri yapılan değişiklikler, bir derleme adımı gerek kalmadan otomatik olarak güncelleştirilir. Kaydet ve güncelleştirme görüntülemek için tarayıcıyı yenilemeniz değişikliği yapın.  

Kod dosyaları yapılan değişiklikler, derleme ve Kestrel kapsayıcı içinde bir yeniden başlatma gerektirir. Değişikliği yaptıktan sonra kapsayıcı içinde uygulamayı başlatın ve işlemi gerçekleştirmek için CTRL + F5 kullanın. Docker kapsayıcısı yeniden oluşturulduğunda veya durdurulmuş değil. Çalıştırma `docker ps` PMC komutu. Özgün kapsayıcı itibariyle 10 dakika önce hala çalışıyor dikkat edin:

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Docker görüntüleri yayımlama

Uygulamanızı geliştirme ve hata ayıklama döngüsünü tamamladıktan sonra Docker için Visual Studio Araçları uygulamanızı üretim görüntüsü oluşturmanıza yardımcı olması. Aşağı açılan yapılandırmasını değiştirme **sürüm** ve uygulama oluşturun. Araç ile görüntü üretiyor *son* özel kayıt defteri ya da Docker hub'a anında etiketi. 

Çalıştırma `docker images` görüntüleri listesini görmek için PMC komutunu:

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` Komut deposu adları ara görüntülerle döndürür ve etiketleri tanımlanan olarak  *\<Hiçbiri >* (Yukarıda listelenmeyen). Bu görüntüleri adlandırılmamış tarafından üretilen [çok aşama yapı](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*. Son görüntü oluşturmanın verimliliğini artırmak&mdash;değişiklik olduğunda yalnızca gerekli Katmanlar yeniden oluşturulur. Aracı görüntüleri artık gerektiğinde, bunları silin kullanarak [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.

Karşılaştırma için boyutu daha küçük olacak şekilde üretim veya yayın görüntüsü için bir Beklenti olabilir *geliştirme* görüntü. Hata ayıklayıcı ve uygulama birimi eşlemenin nedeniyle yerel makinenize ve kapsayıcıdaki çalışıyordu. *Son* görüntü bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli uygulama kodu paketlenmiş. Bu nedenle, delta uygulama kodunuzun boyutudur.
