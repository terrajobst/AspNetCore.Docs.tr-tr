---
title: "ASP.NET çekirdeği ile Docker için Visual Studio Araçları"
description: "Bu makalede, Visual Studio 2017 araçları ve Windows için Docker ASP.NET Core uygulamanın containerize kullanmayı kılavuzluk."
keywords: "Docker,ASP.NET çekirdek, Visual Studio, kapsayıcı"
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a>Docker için Visual Studio Araçları

[Microsoft Visual Studio 2017](https://www.visualstudio.com/) ile [Windows için Docker](https://docs.docker.com/docker-for-windows/install/) oluşturma, hata ayıklama ve çalıştırma Windows ve Linux kapsayıcıları kullanarak .NET Framework ve .NET Core web ve konsol uygulamaları destekler.

## <a name="prerequisites"></a>Önkoşullar

- [Microsoft Visual Studio 2017](https://www.visualstudio.com/) .NET Core iş yükü ile
- [Windows için docker](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>Yükleme ve Kurulum

Yükleme [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) .NET Core iş yükü ile. Visual Studio yüklüyse zaten varsa, [Visual Studio yüklemenizin değiştirme](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) .NET Core iş yükü eklemek için.

Docker yükleme için bilgileri gözden geçirmeniz [Windows için Docker: yüklemeden önce bilmeniz gerekenler](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) yükleyip [Docker için Windows](https://docs.docker.com/docker-for-windows/install/).

Kurulum için gerekli yapılandırmadır  **[paylaşılan sürücüleri](https://docs.docker.com/docker-for-windows/#shared-drives)**  Windows için Docker. Ayar birim eşleme ve hata ayıklama desteği için gereklidir.

Sistem tepsisindeki Docker simgesine sağ tıklayın, **ayarları**seçip **paylaşılan sürücüleri**. Docker burada ve dosyalarınızı depolamak değişiklikleri uygulamak sürücüyü seçin.

![Paylaşılan sürücüler](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a>ASP.NET Web uygulaması oluşturma ve Docker desteği ekleme

Visual Studio kullanarak, yeni bir ASP.NET çekirdek Web uygulaması oluşturun. Uygulama yüklendiğinde ya da seçin **Docker destek eklemek** gelen **proje menüsü** ya da Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**  >  **Docker Destek**.

*Proje menüsü*

![Proje Docker desteği ekleme](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

*Proje bağlam menüsü*

![Sağ tıklatın, Docker desteği ekleme](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

Projeniz için Docker desteği eklediğinizde, Windows veya Linux kapsayıcıları seçebilirsiniz. (Docker ana bilgisayar aynı kapsayıcı türü çalıştırması gerekir. Kapsayıcı türü çalışan Docker örneğinde değiştirirseniz sağ **Docker** sistem tepsisi simgesi ve seçin **geçiş Windows kapsayıcılara** veya **geçiş için Linux kapsayıcıları**.) 

Aşağıdaki dosyaları projeye eklendi:

- **Dockerfile**: ASP.NET Core uygulamaları için Docker dosyasını dayanır [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) görüntü. Bu görüntü öncesi jitted iyileştirme başlangıç performans silinmiş ASP.NET Core NuGet paketlerini içerir. .NET Core konsol uygulamaları oluştururken, Dockerfile gelen en son başvurur [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) görüntü.   
- **docker-compose.yml**: yerleşik ve birlikte çalışma docker görüntülerin koleksiyonunu tanımlamak için kullanılan temel Docker Compose dosya-yapı/Çalıştır oluşturun.   
- **docker compose.dev.debug.yml**: ek yapılandırmanızı hata ayıklamak için ayarlandığında dosyasıyla yinelemeli değişiklikler için docker-oluşturun. Visual Studio -f docker-compose.yml - f docker-bu arada birleştirmek için compose.dev.debug.yml çağırır. Bu oluşturma dosya Visual Studio geliştirme araçları tarafından kullanılır.   
- **docker compose.dev.release.yml**: sürüm tanımınızı hata ayıklamak için ek Docker Compose dosya. İçinde birim bağlama hata ayıklayıcı üretim görüntü içeriğini değiştirmez şekilde.  

*Docker-compose.yml* dosya Projeyi çalıştırdığınızda oluşturduğunuz görüntünün adını içerir. 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

Bu örnekte, `image: user/hellodockertools${TAG}` görüntü oluşturur `user/hellodockertools:dev` uygulama çalıştırıldığında **hata ayıklama** modu ve `user/hellodockertools:latest` içinde **sürüm** modu sırasıyla. 

Değiştirmek istediğiniz `user` için [Docker hub'a](https://hub.docker.com/) kayıt defterine Görüntü göndermeyi planlıyorsanız, kullanıcı adı. Örneğin, `spboyer/hellodockertools`, veya özel kayıt defteri URL'nizi değiştirme `privateregistry.domain.com/` yapılandırmanıza bağlı olarak.

### <a name="debugging"></a>Hata Ayıklama

Seçin **Docker** hata ayıklama açılır araç çubuğu ve kullanım uygulama hata ayıklamayı başlatmak için F5'ndan. 

- *Microsoft/aspnetcore* görüntü edinilen (henüz önbelleğindeki, varsa)
- *ASPNETCORE_ENVIRONMENT* geliştirme kapsayıcı içinde ayarlama
- Bağlantı noktası 80 GÖSTERİLİR ve localhost için dinamik olarak atanmış bir bağlantı noktası eşlenir. Bağlantı noktası docker ana bilgisayarı tarafından belirlenir ve docker ps ile sorgulanabilir. 
- Uygulamanız için kapsayıcı kopyalanır.
- Varsayılan tarayıcı dinamik olarak atanan bağlantı noktasını kullanan kapsayıcıya hata ayıklayıcısı ekli başlatılır. 

Yerleşik elde edilen Docker görüntü *geliştirme* uygulamanızla görüntüsü *microsoft/aspnetcore* temel görüntü olarak görüntüler.

**Not:** hata ayıklama yapılandırmaları yinelemeli deneyimi sağlamak için birim bağlama kullandıkça geliştirme görüntü, uygulama içeriğini boştur. Bir görüntü göndermek için yayın yapılandırmasını kullanın.

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

Uygulamayı çalıştırarak görebilirsiniz kapsayıcısı kullanarak çalıştıran `docker ps` komutu.

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a>Düzenle ve Devam Et

Statik dosyaları ve/veya bir razor şablon dosyalarını değiştirir (*.cshtml*) bir derleme adımı gerek olmadan otomatik olarak güncelleştirilir. Kaydet ve güncelleştirme görüntülemek için tarayıcıda yenileme dokunun değişikliği yapın.  

Kod dosyaları yapılan değişiklikler, derleme ve Kestrel kapsayıcı içinde bir yeniden başlatma gerektirir. Değişikliği yaptıktan sonra işlemi gerçekleştirmek ve kapsayıcı içinde uygulamayı başlatmak için CTRL + F5 kullanın. Docker kapsayıcısı yeniden oluşturulduğunda veya durdurulmuş değil; kullanarak `docker ps` komut satırında, özgün kapsayıcı itibariyle 10 dakika önce hala çalıştığını görebilirsiniz. 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a>Yayımlama Docker görüntüleri

Uygulamanızı geliştirme ve hata ayıklama döngüsünü tamamladıktan sonra Docker için Visual Studio Araçları uygulamanızı üretim görüntüsünü oluşturmak yardımcı olur. Hata ayıklama açılır değiştirme **sürüm** ve uygulamayı yapılandırın. Araç görüntüyle üretecektir `:latest` özel kayıt defteri ya da Docker hub'a anında etiketi. 

Kullanarak `docker images` komutu, görüntüleri listesini görebilirsiniz.

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

Karşılaştırma için boyutu daha küçük olacak şekilde üretim veya yayın görüntüsü için bir Beklenti olabilir **geliştirme** ; ancak, birimi eşlemenin kullanımı ile uygulama ve hata ayıklayıcı gerçekte yerel bilgisayardan çalıştırılmakta görüntüsü Makine kapsayıcısı içinde değil ve. **Son** görüntü bir konak makinesi üzerinde uygulamayı çalıştırmak için gerekli tüm uygulama kodu paketlenmiş, bu nedenle, delta uygulama kodunuz boyutudur.
