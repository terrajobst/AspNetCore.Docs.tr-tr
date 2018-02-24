---
title: "Tepki proje şablonu kullanın"
author: SteveSandersonMS
description: "Tepki ve oluşturma tepki-uygulama için ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile başlayacağınızı öğrenin."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: e093a47159fd8278ff3705bb2c53571a8e27fab8
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-react-project-template"></a>Tepki proje şablonu kullanın

> [!NOTE]
> Bu belge hakkında tepki proje şablonu ASP.NET Core 2. 0 ' dahil değildir. Bunu el ile güncelleştirme yapabilmeniz için daha yeni tepki hakkında şablonudur. Şablon ASP.NET Core 2.1 içinde varsayılan olarak dahil edilir.

Güncelleştirilmiş tepki proje şablonu uygun bir başlama noktası için ASP.NET Core tepki kullanarak uygulamaları sağlar ve [oluşturma tepki-uygulama](https://github.com/facebookincubator/create-react-app) (CRA) kuralları zengin, istemci tarafı kullanıcı arabirimini (UI).

Şablon, hem bir API arka ucu olarak görev yapması için bir ASP.NET Core projesi hem de bir kullanıcı Arabirimi, ancak her ikisinde de yerleşik ve tek bir birim olarak yayımlanan bir tek uygulama projesi barındırma kullanım görev yapması için standart bir CRA tepki proje oluşturmak için eşdeğerdir.

## <a name="create-a-new-app"></a>Yeni uygulama oluştur

ASP.NET Core 2.0 kullanıyorsanız, seçtiğiniz olun [güncelleştirilmiş tepki proje şablonu yüklü](xref:spa/index#installation). ASP.NET Core 2.1 varsa, yüklemek için gerek yoktur.

Komutunu kullanarak bir komut isteminden yeni bir proje oluşturun `dotnet new react` boş bir dizinde. Örneğin, aşağıdaki komutları uygulaması oluşturmak bir *-yeni-Uygulamam* dizini ve bu dizine geçin:

```console
dotnet new react -o my-new-app
cd my-new-app
```

Uygulama, Visual Studio veya .NET Core CLI çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Oluşturulan açmak *.csproj* dosya ve uygulama buradan normal olarak çalıştırın.

Oluşturma işlemi birkaç dakika sürebilir ilk çalıştırmada npm bağımlılıkları yükler. Sonraki derlemeleri çok daha hızlıdır.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Adlı bir ortam değişkeni olduğundan emin olun `ASPNETCORE_Environment` değeriyle `Development`. (İçinde olmayan PowerShell komut istemleri) Windows üzerinde çalışan `SET ASPNETCORE_Environment=Development`. Linux veya macOS, çalıştırılan `export ASPNETCORE_Environment=Development`.

Çalıştırma `dotnet build` uygulamanızı doğrulamak için derlemeler doğru. İlk çalıştırılmasında oluşturma işlemi birkaç dakika sürebilir npm bağımlılıkları yükler. Sonraki derlemeleri çok daha hızlıdır.

Çalıştırma `dotnet run` uygulamayı başlatmak için.

---

Proje şablonu, bir ASP.NET Core uygulama ve tepki bir uygulama oluşturur. ASP.NET Core uygulama veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır. Bulunan tepki uygulama *ClientApp* alt, tüm kullanıcı Arabirimi sorunları için kullanılmak üzere tasarlanmıştır.

## <a name="add-pages-images-styles-modules-etc"></a>Sayfaları, görüntüler, stil, modüller, vb. ekleyin.

*ClientApp* standart CRA tepki uygulama dizindir. Resmi görmek [CRA belgelerine](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) daha fazla bilgi için.

Bu şablon tarafından oluşturulan tepki uygulaması ile CRA tarafından oluşturulan arasında küçük farklar vardır; Ancak, uygulama özelliklerini farklı değildir. Şablon tarafından oluşturulan uygulama içeren bir [önyükleme](https://getbootstrap.com/)-tabanlı düzeni ve temel bir yönlendirme örneği.

## <a name="install-npm-packages"></a>Npm paket yüklemek için

Üçüncü taraf npm paketleri yüklemek için bir komut isteminde kullanın *ClientApp* alt dizin. Örneğin:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Yayımlama ve dağıtma

Geliştirme, uygulama geliştiriciye kolaylık sağlamak için en iyi hale getirilmiş bir modda çalışır. Örneğin, (hata ayıklama sırasında özgün kaynak kodunuzu görebilmeniz için) kaynak eşlemeleri JavaScript paketleri içerir. Uygulama JavaScript, HTML ve CSS değişiklikleri diskte dosya izleyen ve otomatik olarak yeniden derler ve bu dosyaları değiştirmek gördüğünde yeniden yükler.

Üretimde, uygulamanızın performansını en iyi duruma getirilmiş sürüm hizmet. Bu, otomatik olarak gerçekleşecek şekilde yapılandırılır. Derleme yapılandırması yayımladığınızda, istemci-tarafı kodunuzu küçültülmüş, transpiled derlemesinde yayar. Geliştirme yapı farklı olarak, üretim yapı Node.js sunucu üzerinde yüklü olmasını gerektirmez.

Standart kullanabilirsiniz [ASP.NET Core barındırma ve dağıtma yöntemleri](xref:host-and-deploy/index).

## <a name="run-the-cra-server-independently"></a>CRA sunucunun bağımsız olarak çalıştırın

Proje ASP.NET Core uygulama geliştirme modunda başlatıldığında kendi örneğini CRA geliştirme sunucusu, arka planda başlatmak için yapılandırılır. Ayrı bir sunucu el ile çalıştırmayı gerekmediği anlamına gelir kolay olmasıdır.

Bu varsayılan kurulum bir dezavantajı yoktur. C# kodunuzu ve ASP.NET uygulama yeniden başlatılmalıdır çekirdek her değiştirdiğinizde CRA sunucuyu yeniden başlatır. Birkaç saniye yeniden başlatılması gerekir. Sık C# kod düzenlemeleri yapıyorsanız ve CRA sunucunun yeniden başlatılmasını beklemek istemiyorsanız, harici olarak CRA sunucunun bağımsız olarak ASP.NET Core işlemini çalıştırın. Bunu yapmak için:

1. Bir komut isteminde, geçiş *ClientApp* alt ve başlatma CRA geliştirme sunucusu:

    ```console
    cd ClientApp
    npm start
    ```

2. Aşağıdakilerden birini kendi başlatmayı yerine dış CRA sunucu örneği kullanmak için ASP.NET Core uygulamanızı değiştirin. İçinde *başlangıç* sınıfı, yerine `spa.UseReactDevelopmentServer` aşağıdaki çağırmayla:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

ASP.NET Core uygulamanızı başlattığınızda CRA sunucu başlatma olmaz. El ile başlatılan örnek yerine kullanılır. Bu, başlatma ve hızlı yeniden sağlar. Artık her zaman yeniden oluşturmak tepki uygulamanız için bekliyor.
