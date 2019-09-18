---
title: IIS 'de ASP.NET Core uygulaması yayımlama
author: guardrex
description: ASP.NET Core uygulamasının bir IIS sunucusunda nasıl barındırılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: tutorials/publish-to-iis
ms.openlocfilehash: 4ac7b3a2f738e443263dd888f556e0aff7c8099b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082374"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a>IIS 'de ASP.NET Core uygulaması yayımlama

Tarafından [Luke Latham](https://github.com/guardrex)

Bu öğreticide bir IIS sunucusunda ASP.NET Core uygulamasının nasıl barındırılacağını gösterilmektedir.

Bu öğreticide aşağıdaki konular ele alınmaktadır:

> [!div class="checklist"]
> * .NET Core barındırma paketi 'ni Windows Server 'a yükler.
> * IIS Yöneticisi 'nde bir IIS sitesi oluşturun.
> * ASP.NET Core uygulamasını dağıtın.

## <a name="prerequisites"></a>Önkoşullar

* Geliştirme makinesinde yüklü [.NET Core SDK](/dotnet/core/sdk) .
* **Web sunucusu (IIS)** sunucu rolüyle yapılandırılmış Windows Server. Sunucunuz Web sitelerini IIS ile barındırmak üzere yapılandırılmamışsa, <xref:host-and-deploy/iis/index#iis-configuration> makalenin *IIS yapılandırması* bölümündeki yönergeleri izleyin ve ardından Bu öğreticiye geri dönün.

> [!WARNING]
> **IIS yapılandırması ve Web sitesi güvenliği, bu öğretici kapsamında olmayan kavramları içerir.** IIS 'de üretim uygulamalarını barındırmadan önce, [MICROSOFT IIS BELGELERINDEKI](https://www.iis.net/) IIS KıLAVUZUNA ve [IIS ile barındırma hakkında ASP.NET Core makalesine](xref:host-and-deploy/iis/index) başvurun.
>
> Bu öğreticide kapsanmayan IIS barındırması için önemli senaryolar şunlardır:
>
> * [ASP.NET Core veri koruması için bir kayıt defteri kovanı oluşturma](xref:host-and-deploy/iis/index#data-protection)
> * [Uygulama havuzunun Access Control listesi (ACL) yapılandırması](xref:host-and-deploy/iis/index#application-pool-identity)
> * IIS dağıtım kavramlarına odaklanmak için bu öğretici, IIS 'de HTTPS güvenliği olmayan bir uygulama dağıtır. HTTPS protokolü için etkinleştirilmiş bir uygulamayı barındırma hakkında daha fazla bilgi için, bu makalenin [ek kaynaklar](#additional-resources) bölümündeki güvenlik konularına bakın. ASP.NET Core uygulamalar barındırılmasına yönelik daha fazla rehberlik <xref:host-and-deploy/iis/index> makalesinde sunulmaktadır.

## <a name="install-the-net-core-hosting-bundle"></a>Paket barındırma .NET Core'u yükleme

*.NET Core barındırma paketi* 'ni IIS sunucusuna yükler. .NET Core çalışma zamanı, .NET Core kitaplığı paketi yükler ve [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module). Modül IIS çalıştırılacak uygulamaları ASP.NET Core sağlar. Sistem, Internet bağlantısı yoksa, alma ve yükleme [Microsoft Visual C++ 2015 yeniden dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=53840) .NET Core barındırma paketini yüklemeden önce.

Aşağıdaki bağlantıyı kullanarak yükleyiciyi indirin:

[Geçerli .NET Core barındırma Paket Yükleyici (doğrudan indirme)](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. Yükleyiciyi IIS sunucusunda çalıştırın.

1. Sunucuyu yeniden başlatın ya da **net stop idi** ' i ve ardından bir komut kabuğu 'ndan **net start w3svc** ' i çalıştırın.

## <a name="create-the-iis-site"></a>IIS sitesi oluştur

1. IIS sunucusunda, uygulamanın yayımlanan klasörlerini ve dosyalarını içeren bir klasör oluşturun. Aşağıdaki adımda, klasörün yolu, uygulamanın fiziksel yolu olarak IIS 'ye sağlanır.

1. IIS Yöneticisi 'nde, **Bağlantılar** panelinde sunucunun düğümünü açın. Sağ **siteleri** klasör. Seçin **Web sitesi Ekle** bağlam menüsünde.

1. Bir **site adı** belirtin ve **fiziksel yolu** , oluşturduğunuz uygulamanın dağıtım klasörüne ayarlayın. **Bağlama** yapılandırmasını sağlayın ve **Tamam**' ı seçerek Web sitesini oluşturun.

## <a name="create-an-aspnet-core-razor-pages-app"></a>ASP.NET Core Razor Pages uygulaması oluşturma

Razor Pages uygulaması oluşturmak için öğreticiyiizleyin.<xref:getting-started>

## <a name="publish-and-deploy-the-app"></a>Uygulamayı yayımlama ve dağıtma

*Uygulama yayımlama* , bir sunucu tarafından barındırılabilecek derlenmiş bir uygulama oluşturmak anlamına gelir. *Uygulama dağıtma* , yayımlanan uygulamayı bir barındırma sistemine taşımak anlamına gelir. Yayımla adımı [.NET Core SDK](/dotnet/core/sdk)tarafından işlenir, ancak dağıtım adımı çeşitli yaklaşımlar tarafından işlenebilir. Bu öğretici, şu konumda *klasör* dağıtım yaklaşımını benimsemektedir:

* Uygulama bir klasöre yayımlanır.
* Klasörün içeriği IIS sitesinin klasörüne taşınır (IIS Yöneticisi 'nde sitenin **fiziksel yolu** ).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **Çözüm Gezgini** projede projeye sağ tıklayın ve **Yayımla**' yı seçin.
1. **Bir yayımlama hedefi seç** iletişim kutusunda, **klasörü** Yayımla seçeneğini belirleyin.
1. **Klasör veya dosya paylaşma** yolunu ayarlayın.
   * Geliştirme makinesinde bir ağ paylaşımında bulunan IIS sitesi için bir klasör oluşturduysanız, paylaşımın yolunu belirtin. Geçerli kullanıcının paylaşıma yayımlamak için yazma erişimi olmalıdır.
   * IIS sunucusunda IIS site klasörüne doğrudan dağıtım yapadıysanız, kaldırılabilir medyada bir klasöre yayımlayın ve yayımlanan uygulamayı sunucuda, sitenin **fiziksel yolu** olan sunucudaki IIS site klasörüne fiziksel olarak taşıyın. *Bin/Release/{Target Framework}/Publish* klasörünün IÇERIĞINI sunucusundaki IIS site klasörüne taşıyın, bu site, sitenin IIS Yöneticisi 'ndeki **fiziksel yoludur** .

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Bir komut kabuğunda, uygulamayı sürüm yapılandırması 'nda [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutuyla yayımlayın:

   ```dotnetcli
   dotnet publish --configuration Release
   ```

1. *Bin/Release/{Target Framework}/Publish* klasörünün IÇERIĞINI sunucusundaki IIS site klasörüne taşıyın, bu site, sitenin IIS Yöneticisi 'ndeki **fiziksel yoludur** .

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. **Çözümdeki** projeye sağ tıklayın ve**Yayımla klasörünü** **Yayımla ' yı seçin.**  > 
1. **Klasör seçin** yolunu ayarlayın.
   * Geliştirme makinesinde bir ağ paylaşımında bulunan IIS sitesi için bir klasör oluşturduysanız, paylaşımın yolunu belirtin. Geçerli kullanıcının paylaşıma yayımlamak için yazma erişimi olmalıdır.
   * IIS sunucusunda IIS site klasörüne doğrudan dağıtım yapadıysanız, kaldırılabilir medyada bir klasöre yayımlayın ve yayımlanan uygulamayı sunucuda, sitenin **fiziksel yolu** olan sunucudaki IIS site klasörüne fiziksel olarak taşıyın. *Bin/Release/{Target Framework}/Publish* klasörünün IÇERIĞINI sunucusundaki IIS site klasörüne taşıyın, bu site, sitenin IIS Yöneticisi 'ndeki **fiziksel yoludur** .

---

## <a name="browse-the-website"></a>Web sitesine Gözat

Uygulamanın ilk isteği aldıktan sonra tarayıcıda erişilebilir olması gerekir. Site için IIS Yöneticisi 'nde oluşturduğunuz uç nokta bağlamasındaki uygulamaya bir istek oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * .NET Core barındırma paketi 'ni Windows Server 'a yükler.
> * IIS Yöneticisi 'nde bir IIS sitesi oluşturun.
> * ASP.NET Core uygulamasını dağıtın.

IIS 'de ASP.NET Core uygulamaları barındırma hakkında daha fazla bilgi için bkz. IIS genel bakış makalesi:

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a>Ek kaynaklar

### <a name="articles-in-the-aspnet-core-documentation-set"></a>ASP.NET Core belge kümesi makaleleri

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a>ASP.NET Core Uygulama dağıtımıyla ilgili makaleler

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [Mac için Visual Studio kullanarak bir Web uygulamasını bir klasöre yayımlama](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a>IIS HTTPS yapılandırması makaleleri

* [IIS Yöneticisi 'nde SSL 'yi yapılandırma](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [IIS 'de SSL ayarlama](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a>IIS ve Windows Server makaleleri

* [Resmi Microsoft IIS sitesi](https://www.iis.net/)
* [Windows Server Teknik İçerik Kitaplığı](/windows-server/windows-server)
