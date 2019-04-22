---
title: Visual Studio Code ile azure'da ASP.NET Core uygulaması yayımlama
author: ricardoserradas
description: Visual Studio Code kullanarak Azure App Service'e bir ASP.NET Core uygulaması yayımlama hakkında bilgi edinin
ms.author: ricardoserradas
ms.custom: mvc
ms.date: 04/16/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: 64d82835f6a47a458802692c99658b964c07f807
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59711557"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a>Visual Studio Code ile azure'da ASP.NET Core uygulaması yayımlama

Tarafından [Ricardo Serradas](https://twitter.com/ricardoserradas)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

App Service dağıtım sorun gidermek için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="intro"></a>Giriş

Bu öğreticide bir ASP.Net Core MVC uygulaması oluşturun ve Visual Studio Code içinde dağıtma öğreneceksiniz.

## <a name="set-up"></a>Ayarlama

- Açık bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/dotnet/) tane yoksa.
- Yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download)
- Yükleme [Visual Studio kodu](https://code.visualstudio.com/Download)
  - Yükleme [ C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio code'da
  - Tümünü Yükle [Azure uygulama hizmeti uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) Visual Studio Code için ve devam etmeden önce yapılandırma

## <a name="create-an-aspnet-core-mvc-project"></a>Bir ASP.Net Core MVC projesi oluşturma

Bir terminal kullanarak klasöre gidin, istediğiniz proje üzerinde oluşturulmuş ve aşağıdaki komutu kullanın:

```cmd
> dotnet new mvc
```

Aşağıdakine benzer bir klasör yapısı vardır:

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a>Visual Studio Code ile Aç

Projeniz oluşturulduktan sonra aşağıdaki seçeneklerden birini kullanarak Visual Studio Code ile açabilirsiniz:

### <a name="through-the-command-line"></a>Komut satırı aracılığıyla

Projeyi oluşturduğunuzda, klasörde aşağıdaki komutu kullanın:

```cmd
> code .
```

Aşağıdaki komut çalışmazsa başvurarak yüklemenizi düzgün şekilde yapılandırıldığını denetlemek [bu bağlantıyı](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).

### <a name="through-visual-studio-code-interface"></a>Visual Studio Code arabirimi aracılığıyla

- Açık Visual Studio kodu
- Menüde seçin `File > Open Folder`
- Kök seçin MVC projesini oluşturduğunuz klasörü

Proje klasörü açtığınızda, derleme ve hata ayıklamak için gerekli varlıkları eksik olduğunu bildiren bir ileti alırsınız. Bunları eklemek için Yardım'ı kabul edin.

![Proje yüklendi ile Visual Studio Code arabirimi](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

A `.vscode` klasör Proje yapısı altında oluşturulur. Aşağıdaki dosyaları içerir:

```cmd
launch.json
tasks.json
```

Bu yapı ve .NET Core Web uygulamanızı hata ayıklama yardımcı olması için yardımcı programı dosyalarıdır.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Biz uygulamayı Azure'a dağıtmadan önce yerel makinenizde düzgün biçimde çalıştığından emin olun.

- Projeyi çalıştırmak için F5 tuşuna basın

Web uygulamanızı çalıştıran yeni bir varsayılan tarayıcı sekmesinde başlar. Başladıktan hemen sonra bir Gizlilik uyarısı fark edebilirsiniz. Ya da uygulamanızı başlatacak nedeni HTTP ve HTTPS ve kullanarak, HTTPS uç noktasına varsayılan olarak gider.

![Uygulamayı yerel olarak hata ayıklama sırasında Gizlilik uyarısı](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

Hata ayıklama oturumu korumak için tıklayın `Advanced` ardından `Continue to localhost (unsafe)`.

## <a name="generate-the-deployment-package-locally"></a>Yerel olarak dağıtım paketi oluşturun

- Visual Studio Code Terminali açın
- Oluşturmak için aşağıdaki komutu kullanın. bir `Release` adlı bir alt klasörü paketine `publish`:
  - `dotnet publish -c Release -o ./publish`
- Yeni bir `publish` klasör Proje yapısı altında oluşturulacak

![Klasör yapısını yayımlama](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a>Azure App Service’e yayımlama

Visual Studio Code için Azure App Service uzantısı yararlanarak, doğrudan Azure App Service Web sitesine yayımlamak için aşağıdaki adımları izleyin.

### <a name="if-youre-creating-a-new-web-app"></a>Yeni bir Web uygulaması oluşturuyorsanız

- Sağ tıklayın `publish` klasörü ve seçin `Deploy to Web App...`
- Web uygulamasını oluşturmak istediğiniz aboneliği seçin
- Seçin `Create New Web App`
- Web uygulaması için bir ad girin

Uzantı, yeni Web uygulaması oluşturur ve paket dağıttıktan otomatik olarak başlatılacak. Dağıtım tamamlandıktan sonra tıklayın `Browse Website` dağıtımını doğrulamak için.

![Dağıtım başarılı iletisi](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

' A tıkladığınızda `Browse Website`, varsayılan tarayıcınızı kullanarak ona gitmeniz:

![Yeni Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a>Mevcut bir Web uygulamasına dağıtım yapıyorsanız

- Sağ tıklayın `publish` klasörü ve seçin `Deploy to Web App...`
- Mevcut Web uygulamasının bulunduğu abonelik seçin
- Web uygulaması listeden seçin.
- Visual Studio Code varolan içeriğin üzerine yazmak isteyip istemediğinizi sorar. Tıklayın `Deploy` onaylamak için

Uzantının güncelleştirilmiş içeriği Web uygulamasına dağıtır. Bunu yaptıktan sonra tıklayın `Browse Website` dağıtımını doğrulamak için.

![Mevcut Web uygulaması başarıyla dağıtıldı](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure DevOps işlem hattınızı oluşturun](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure uygulama hizmeti](/azure/app-service/app-service-web-overview)
- [Azure kaynak grupları](/azure/azure-resource-manager/resource-group-overview#resource-groups)