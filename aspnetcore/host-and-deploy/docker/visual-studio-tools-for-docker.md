---
title: ASP.NET Core ile Docker için Visual Studio Araçları
author: spboyer
description: Bir ASP.NET Core uygulamasını kapsayıcılı hale getirme için Visual Studio 2017 araçları ve Docker için Windows kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 3bf3d8d0a627d97090e3ce9fef7e380f03c7626d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901310"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>ASP.NET Core ile Docker için Visual Studio Araçları

Visual Studio 2017, oluşturma, hata ayıklama ve kapsayıcılı ASP.NET Core .NET Core'u hedefleyen uygulamaların çalıştırılmasını destekler. Hem Windows hem de Linux kapsayıcıları desteklenmektedir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/docker/visual-studio-tools-for-docker/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

* [Windows için docker](https://docs.docker.com/docker-for-windows/install/)
* [Visual Studio 2017](https://visualstudio.microsoft.com) ile **.NET Core çoklu platform geliştirme** iş yükü

## <a name="installation-and-setup"></a>Yükleme ve Kurulum

Docker yükleme için bölümündeki bilgileri gözden geçirin [için Docker Windows: Yüklemeden önce bilinmesi gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install). Ardından, yükleme [için Docker Windows](https://docs.docker.com/docker-for-windows/install/).

**[Sürücüleri paylaşılan](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker birimi eşlemenin ve hata ayıklamayı destekleyecek şekilde yapılandırılması gerekir. Sistem tepsisi'nın Docker simgesine sağ tıklayın, **ayarları**seçip **paylaşılan sürücüleri**. Docker dosyaları depoladığı sürücü seçin. **Uygula**'ya tıklayın.

![Kapsayıcılar için paylaşımı yerel C sürücüsüne seçmek için iletişim kutusu](visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 sürüm 15.6 ve daha sonra sor **paylaşılan sürücüleri** yapılandırılmamışlardır.

## <a name="add-a-project-to-a-docker-container"></a>Bir Docker kapsayıcısı için bir proje ekleyin

Bir ASP.NET Core projesi kapsayıcılı hale getirme için projenin .NET Core hedeflemesi gerekir. Hem Linux hem de Windows kapsayıcıları desteklenmektedir.

Bir projeye Docker desteği eklenirken bir Windows veya Linux kapsayıcısı seçin. Docker konağı aynı kapsayıcı türü çalıştırmalıdır. Çalışan Docker örneğinde kapsayıcı türü değiştirmek için sistem tepsisindeki'nın Docker simgesini sağ tıklatın ve seçin **Windows kapsayıcılarına geç...**  veya **geçiş Linux kapsayıcıları için...** .

### <a name="new-app"></a>Yeni uygulama

İle yeni bir uygulama oluştururken **ASP.NET Core Web uygulaması** proje şablonları, select **Docker desteğini etkinleştir** onay kutusunu:

![Docker desteği onay kutusunu etkinleştirin](visual-studio-tools-for-docker/_static/enable-docker-support-check-box.png)

Hedef çerçeveyi .NET Core ise **işletim sistemi** açılan bir kapsayıcı türünün seçimini sağlar.

### <a name="existing-app"></a>Mevcut uygulama

.NET Core'u hedefleyen ASP.NET Core projeleri için araç kullanımı aracılığıyla Docker desteği eklemek için iki seçenek vardır. Projeyi Visual Studio'da açın ve aşağıdaki seçeneklerden birini belirleyin:

* Seçin **Docker desteği** gelen **proje** menüsü.
* Projeye sağ **Çözüm Gezgini** seçip **Ekle** > **Docker desteği**.

Docker için Visual Studio Araçları, .NET Framework'ü hedefleyen var olan bir ASP.NET Core projesine Docker ekleme desteklemez.

## <a name="dockerfile-overview"></a>Dockerfile genel bakış

A *Dockerfile*, son bir Docker görüntüsü oluşturmak için tarif, proje kök dizinine eklenir. Başvurmak [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) içindeki komutları anlaşılması için. Bu özellikle *Dockerfile* kullanan bir [çok aşamalı derleme](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) dört olarak ayrı, adlı derleme aşamaları:

::: moniker range=">= aspnetcore-2.1"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile.original?highlight=1,6,14,17)]

Önceki *Dockerfile* dayanır [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet/) görüntü. Bu temel görüntü ASP.NET Core çalışma zamanı ve NuGet paketleri içerir. Just-in-başlangıç performansını artırmak için derlenmiş time (JIT) paketlerdir.

Yeni Proje iletişim kutusunun **HTTPS için Yapılandır** onay kutusunu işaretli *Dockerfile* iki bağlantı noktalarını kullanıma sunar. Bir bağlantı noktası, HTTP trafiği için kullanılır. diğer bağlantı noktasını, HTTPS için kullanılır. Onay kutusu işaretli değilse, HTTP trafiği için tek bir bağlantı noktası (80) sunulur.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-dockerfile[](visual-studio-tools-for-docker/samples/2.0/HelloDockerTools/Dockerfile?highlight=1,5,13,16)]

Önceki *Dockerfile* dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore/) görüntü. Bu temel görüntü yalnızca başlangıç performansını artırmak için derlenmiş zamanında (JIT) olan ASP.NET Core NuGet paketleri içerir.

::: moniker-end

## <a name="add-container-orchestrator-support-to-an-app"></a>Kapsayıcı Düzenleyicisi desteği için uygulama ekleme

Visual Studio 2017 sürüm 15.7 veya önceki Destek [Docker Compose](https://docs.docker.com/compose/overview/) tek kapsayıcı düzenleme çözümüne olarak. Docker Compose yapıtlar aracılığıyla eklenen **Ekle** > **Docker desteği**.

Visual Studio 2017 sürüm 15,8 veya üzeri yalnızca istendiğinde düzenleme çözümüne ekleyin. Projeye sağ **Çözüm Gezgini** seçip **Ekle** > **kapsayıcı Düzenleyicisi desteği**. İki farklı seçenekler sunulur: [Docker Compose](#docker-compose) ve [Service Fabric](#service-fabric).

### <a name="docker-compose"></a>Docker Compose

Docker için Visual Studio Araçları ekleme bir *docker-compose* çözüme aşağıdaki dosyaları ile:

* *docker-compose.dcproj* &ndash; bir projeyi temsil eden dosya. İçeren bir `<DockerTargetOS>` kullanılacak işletim sistemi belirten öğe.
* *.dockerignore* &ndash; bir derleme bağlamı oluşturulurken dışlanacak dosya ve dizin desenleri listeler.
* *docker-compose.yml* &ndash; temel [Docker Compose](https://docs.docker.com/compose/overview/) çalıştırın ve yerleşik görüntü koleksiyonunu tanımlamak için kullanılan dosya `docker-compose build` ve `docker-compose run`sırasıyla.
* *docker-compose.override.yml* &ndash; isteğe bağlı bir dosya okuma Docker Compose, yapılandırma ile Hizmetleri için geçersiz kılar. Visual Studio yürütür `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` bu dosyaları birleştirmek için.

*Docker-compose.yml* dosyasına başvuran Proje çalıştırıldığında, oluşturulan görüntü adı:

[!code-yaml[](visual-studio-tools-for-docker/samples/2.0/docker-compose.yml?highlight=5)]

Önceki örnekte `image: hellodockertools` görüntü oluşturur `hellodockertools:dev` uygulamayı çalıştırdığında **hata ayıklama** modu. `hellodockertools:latest` Görüntü uygulama çalıştırıldığında oluşturulan **yayın** modu.

Görüntü adı ile önek [Docker Hub](https://hub.docker.com/) kullanıcı adı (örneğin, `dockerhubusername/hellodockertools`), görüntünün kayıt defterine gönderilir. Alternatif olarak, özel kayıt defteri URL'si eklemek için görüntü adı değiştirin (örneğin, `privateregistry.domain.com/hellodockertools`) yapılandırmasına bağlı olarak.

Farklı istiyorsanız (örneğin, hata ayıklama veya sürüm), yapı yapılandırmasına göre davranış ekleyin yapılandırmaya özgü *docker-compose* dosyaları. Dosyaları yapı yapılandırmasına göre adlı olmalıdır (örneğin, *docker compose.vs.debug.yml* ve *docker compose.vs.release.yml*) ve aynıkonumayerleştirilir*docker compose override.yml* dosya. 

Yapılandırmaya özgü geçersiz kılma dosyalarını kullanarak, hata ayıklama ve yayın derleme yapılandırmaları için farklı yapılandırma ayarları (örneğin, ortam değişkenleri veya giriş noktası) belirtebilirsiniz.

### <a name="service-fabric"></a>Service Fabric

Temel yanı sıra [önkoşulları](#prerequisites), [Service Fabric](/azure/service-fabric/) düzenleme çözümüne aşağıdaki önkoşulları gerektirir:

* [Microsoft Azure Service Fabric SDK'sı](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK) 2.6 veya sonraki bir sürümü
* Visual Studio 2017'in **Azure geliştirme** iş yükü

Service Fabric, yerel geliştirme kümesinde Windows üzerinde çalışan Linux kapsayıcıları desteklemiyor. Proje zaten bir Linux kapsayıcı kullanıyorsanız, Windows kapsayıcıları için geçiş yapmak için Visual Studio ister.

Docker için Visual Studio Araçları, şu görevleri yapın:

* Ekler bir  *&lt;project_name&gt;uygulama* **Service Fabric uygulaması** çözüme bir proje.
* Ekler bir *Dockerfile* ve *.dockerignore* dosyasına ASP.NET Core projesi. Varsa bir *Dockerfile* zaten ASP.NET Core proje için adlandırılır *Dockerfile.original*. Yeni bir *Dockerfile*için aşağıdakilere benzer oluşturulur:

    [!code-dockerfile[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/Dockerfile)]

* Ekler bir `<IsServiceFabricServiceProject>` ASP.NET Core proje öğesine *.csproj* dosyası:

    [!code-xml[](visual-studio-tools-for-docker/samples/2.1/HelloDockerTools/HelloDockerTools.csproj?name=snippet_IsServiceFabricServiceProject)]

* Ekler bir *PackageRoot* ASP.NET Core projesi klasör. Yeni hizmet için ayarları ve hizmet bildiriminin klasör içerir.

Daha fazla bilgi için [Azure Service Fabric'e Windows kapsayıcısındaki bir .NET uygulaması dağıtma](/azure/service-fabric/service-fabric-host-app-in-a-container).

## <a name="debug"></a>Hata ayıklama

Seçin **Docker** gelen hata ayıklama açılır araç ve uygulama hata ayıklamayı başlatın. **Docker** görünümünü **çıkış** penceresi, aşağıdaki eylemler alma yeri gösterir:

::: moniker range=">= aspnetcore-2.1"

* *2.1 aspnetcore runtime* etiketi *microsoft/dotnet* çalışma zamanı görüntü alındı (henüz önbellekte değilse). Görüntü ASP.NET Core ve .NET Core çalışma zamanlarını ve ilişkili kitaplıklarını yükler. ASP.NET Core uygulamaları üretim ortamında çalıştırmak için optimize edilmiştir.
* `ASPNETCORE_ENVIRONMENT` Ortam değişkeni ayarlandığında `Development` kapsayıcı içindeki.
* İki dinamik olarak atanan bağlantı noktası sunulur: biri HTTP ve HTTPS için. Localhost için atanan bağlantı noktası ile sorgulanabilir `docker ps` komutu.
* Uygulama, kapsayıcıya kopyalanır.
* Varsayılan tarayıcı hata ayıklayıcısı ekli dinamik olarak atanan bağlantı noktasını kullanarak kapsayıcıya ile başlatılır.

Uygulamasının elde edilen Docker görüntüsü olarak etiketlenmiş *geliştirme*. Görüntü dayanır *2.1 aspnetcore runtime* etiketi *microsoft/dotnet* temel görüntü. Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi. Makine görüntülerinde görüntülenir:

```console
REPOSITORY        TAG                     IMAGE ID      CREATED         SIZE
hellodockertools  dev                     d72ce0f1dfe7  30 seconds ago  255MB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago      255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* *Microsoft/aspnetcore* çalışma zamanı görüntü alındı (henüz önbellekte değilse).
* `ASPNETCORE_ENVIRONMENT` Ortam değişkeni ayarlandığında `Development` kapsayıcı içindeki.
* 80 numaralı bağlantı noktasını kullanıma sunulan ve localhost için dinamik olarak atanan bağlantı noktasına eşlenen. Bağlantı noktası Docker konak tarafından belirlenir ve ile sorgulanabilir `docker ps` komutu.
* Uygulama, kapsayıcıya kopyalanır.
* Varsayılan tarayıcı hata ayıklayıcısı ekli dinamik olarak atanan bağlantı noktasını kullanarak kapsayıcıya ile başlatılır.

Uygulamasının elde edilen Docker görüntüsü olarak etiketlenmiş *geliştirme*. Görüntü dayanır *microsoft/aspnetcore* temel görüntü. Çalıştırma `docker images` komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi. Makine görüntülerinde görüntülenir:

```console
REPOSITORY            TAG  IMAGE ID      CREATED        SIZE
hellodockertools      dev  5fafe5d1ad5b  4 minutes ago  347MB
microsoft/aspnetcore  2.0  c69d39472da9  13 days ago    347MB
```

::: moniker-end

> [!NOTE]
> *Geliştirme* görüntü olarak uygulama içerikleri eksik **hata ayıklama** yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullanın. Görüntü gönderebilmeniz için kullanmak **yayın** yapılandırma.

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

Uygulama geliştirme ve hata ayıklama döngüsünü tamamlandıktan sonra Docker için Visual Studio Araçları, uygulamayı üretim görüntüsü oluşturmada yardımcı olacak. Aşağı açılan yapılandırmasını değiştirmek **yayın** ve bir uygulama geliştirin. Araç, derleme ve yayınlama görüntüyü Docker hub'dan (kullanılmıyorsa zaten önbelleğinde) alır. Görüntü ile üretilen *son* özel kayıt defteri ya da Docker hub'dan gönderilen etiketi.

Çalıştırma `docker images` PMC'yi görüntülerin listesini görüntülemek için komutu. Çıktı aşağıdaki gibi görüntülenir:

::: moniker range=">= aspnetcore-2.1"

```console
REPOSITORY        TAG                     IMAGE ID      CREATED             SIZE
hellodockertools  latest                  e3984a64230c  About a minute ago  258MB
hellodockertools  dev                     d72ce0f1dfe7  4 minutes ago       255MB
microsoft/dotnet  2.1-sdk                 9e243db15f91  6 days ago          1.7GB
microsoft/dotnet  2.1-aspnetcore-runtime  fcc3887985bb  6 days ago          255MB
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```console
REPOSITORY                  TAG     IMAGE ID      CREATED         SIZE
hellodockertools            latest  cd28f0d4abbd  12 seconds ago  349MB
hellodockertools            dev     5fafe5d1ad5b  23 minutes ago  347MB
microsoft/aspnetcore-build  2.0     7fed40fbb647  13 days ago     2.02GB
microsoft/aspnetcore        2.0     c69d39472da9  13 days ago     347MB
```

`microsoft/aspnetcore-build` Ve `microsoft/aspnetcore` yukarıdaki çıktıda listelenen görüntüleri yerine `microsoft/dotnet` itibariyle .NET Core 2.1 görüntüler. Daha fazla bilgi için [Docker depoları geçiş duyuruyu](https://github.com/aspnet/Announcements/issues/298).

::: moniker-end

> [!NOTE]
> `docker images` Depo adları ile Ara görüntü komutu döndürür ve etiketleri tanımlanan olarak  *\<yok >* (Yukarıda listelenmeyen). Bu görüntüleri adlandırılmamış tarafından üretilen [çok aşamalı derleme](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile*. Bunlar, son görüntü oluşturma verimliliğini artırmak&mdash;değişiklikler olduğunda yalnızca gerekli Katmanlar yeniden oluşturulur. Aracı görüntüleri artık gerekli değilse, bunları silin kullanarak [docker RMI](https://docs.docker.com/engine/reference/commandline/rmi/) komutu.

Bir karşılaştırma için boyut olarak daha küçük üretim ya da sürüm resmi beklentisi olabilir *geliştirme* görüntü. Birimi eşlemenin nedeniyle, yerel makine ve kapsayıcı içinde değil hata ayıklayıcı ve uygulamanın çalışıyordu. *Son* görüntü, bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli uygulama kodu paketlenmiş. Bu nedenle, delta uygulama kodu boyutudur.

## <a name="additional-resources"></a>Ek kaynaklar

* [Visual Studio ile kapsayıcı geliştirme](/visualstudio/containers)
* [Azure Service Fabric: Geliştirme ortamınızı hazırlama](/azure/service-fabric/service-fabric-get-started)
* [Azure Service Fabric'e Windows kapsayıcısındaki bir .NET uygulaması dağıtma](/azure/service-fabric/service-fabric-host-app-in-a-container)
* [Visual Studio 2017 geliştirme Docker ile ilgili sorunları giderme](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Docker GitHub deposu için Visual Studio Araçları](https://github.com/Microsoft/DockerTools)
