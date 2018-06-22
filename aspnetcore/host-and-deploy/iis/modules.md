---
title: ASP.NET Core IIS modülleri
author: guardrex
description: ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri bulur.
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 1ff0fdcaae066b493eeebf6a061e383f88c81052
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272743"
---
# <a name="iis-modules-with-aspnet-core"></a>ASP.NET Core IIS modülleri

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamaları IIS tarafından bir ters proxy yapılandırması barındırılır. Bazı yerel IIS modülleri ve tüm yönetilen IIS modülleri ASP.NET Core uygulamaları için istekleri işlemek kullanılamaz. Çoğu durumda, ASP.NET Core IIS yerel ve yönetilen modülleri özelliklerine bir alternatif sunar.

## <a name="native-modules"></a>Yerel modülleri

Tablo ASP.NET Core uygulamaları için ters proxy isteklerde işlevsel yerel IIS modülleri gösterir.

| Modül | ASP.NET Core uygulamaları ile işlev | ASP.NET çekirdeği seçeneği |
| ------ | :-------------------------------: | ------------------- |
| **Anonim kimlik doğrulaması**<br>`AnonymousAuthenticationModule` | Evet | |
| **Temel Kimlik Doğrulaması**<br>`BasicAuthenticationModule` | Evet | |
| **İstemci sertifika eşlemesi kimlik doğrulaması**<br>`CertificateMappingAuthenticationModule` | Evet | |
| **CGI**<br>`CgiModule` | Hayır | |
| **Yapılandırma Doğrulama**<br>`ConfigurationValidationModule` | Evet | |
| **HTTP Hataları**<br>`CustomErrorModule` | Hayır | [Durum kodu sayfaları Ara](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Özel günlüğe kaydetme**<br>`CustomLoggingModule` | Evet | |
| **Varsayılan Belge**<br>`DefaultDocumentModule` | Hayır | [Varsayılan dosya ara yazılımı](xref:fundamentals/static-files#serve-a-default-document) |
| **Özet kimlik doğrulaması**<br>`DigestAuthenticationModule` | Evet | |
| **Dizin Tarama**<br>`DirectoryListingModule` | Hayır | [Dizin tarama Ara](xref:fundamentals/static-files#enable-directory-browsing) |
| **Dinamik sıkıştırma**<br>`DynamicCompressionModule` | Evet | [Yanıt Sıkıştırma Ara Yazılımı](xref:performance/response-compression) |
| **İzleme**<br>`FailedRequestsTracingModule` | Evet | [ASP.NET çekirdeği günlüğü](xref:fundamentals/logging/index#tracesource-provider) |
| **Dosyayı önbelleğe alma**<br>`FileCacheModule` | Hayır | [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware) |
| **HTTP önbelleğe alma**<br>`HttpCacheModule` | Hayır | [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware) |
| **HTTP Günlüğü**<br>`HttpLoggingModule` | Evet | [ASP.NET çekirdeği günlüğü](xref:fundamentals/logging/index)<br>Uygulamaları: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **HTTP Yeniden Yönlendirmesi**<br>`HttpRedirectionModule` | Evet | [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) |
| **IIS istemci sertifika eşlemesi kimlik doğrulaması**<br>`IISCertificateMappingAuthenticationModule` | Evet | |
| **IP ve Etki Alanı Kısıtlamaları**<br>`IpRestrictionModule` | Evet | |
| **ISAPI Filtreleri**<br>`IsapiFilterModule` | Evet | [Ara Yazılım](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Evet | [Ara Yazılım](xref:fundamentals/middleware/index) |
| **Protokol desteği**<br>`ProtocolSupportModule` | Evet | |
| **İstek Filtreleme**<br>`RequestFilteringModule` | Evet | [URL yeniden yazma işlemi Ara `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **İstek İzleyicisi**<br>`RequestMonitorModule` | Evet | |
| **URL yeniden yazma işlemi**<br>`RewriteModule` | Evet&#8224; | [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) |
| **Sunucu Tarafı İçermeler**<br>`ServerSideIncludeModule` | Hayır | |
| **Statik sıkıştırma**<br>`StaticCompressionModule` | Hayır | [Yanıt Sıkıştırma Ara Yazılımı](xref:performance/response-compression) |
| **Statik İçerik**<br>`StaticFileModule` | Hayır | [Statik dosya ara yazılımı](xref:fundamentals/static-files) |
| **Belirteç önbelleğe alma**<br>`TokenCacheModule` | Evet | |
| **URI önbelleği**<br>`UriCacheModule` | Evet | |
| **URL Yetkilendirmesi**<br>`UrlAuthorizationModule` | Evet | [ASP.NET Core kimliği](xref:security/authentication/identity) |
| **Windows kimlik doğrulaması**<br>`WindowsAuthenticationModule` | Evet | |

&#8224;URL yeniden yazma modülün `isFile` ve `isDirectory` türlerle değişiklikleri nedeniyle ASP.NET Core uygulamaları ile çalışmıyor [dizin yapısını](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Yönetilen modüller

Yönetilen modüller *değil* uygulama havuzunun .NET CLR sürümü ayarlandığında barındırılan ASP.NET Core uygulamaları ile işlevsel **yönetilen kod yok**. ASP.NET Core birkaç durumlarda ara yazılımı seçenekleri sunar.

| Modül                  | ASP.NET çekirdeği seçeneği |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Tanımlama bilgisi kimlik doğrulaması ara yazılımı](xref:security/authentication/cookie) |
| OutputCache             | [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Oturum                 | [Oturum Ara](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) |
| UrlRoutingModule 4.0    | [ASP.NET Core kimliği](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS Yöneticisi'ni uygulama değişiklikleri

Ayarları yapılandırmak için IIS Yöneticisi'ni kullanırken *web.config* uygulamanın dosya değiştirilir. Bir uygulama dağıtımı ve de dahil olmak üzere *web.config*, IIS Yöneticisi ile yapılan değişiklikler üzerine tarafından dağıtılan *web.config* dosya. Sunucunun bir değişiklik yaptıysanız *web.config* dosya, güncelleştirilmiş Kopyala *web.config* hemen yerel projeye sunucudaki dosya.

## <a name="disabling-iis-modules"></a>IIS modülleri devre dışı bırakma

Bir IIS modülünü sunucu düzeyinde bir uygulama, bir uygulamanın eklemeyi için devre dışı bırakılmalıdır yapılandırılmışsa *web.config* dosya modül devre dışı bırakabilirsiniz. Modül bırakın ve (varsa) bir yapılandırma ayarını kullanarak devre dışı bırakın ya da uygulamadan modülünü Kaldır.

### <a name="module-deactivation"></a>Modül devre dışı bırakma

Birçok modül uygulamadan modülü kaldırmadan devre dışı bırakılmasına izin veren bir yapılandırma ayarı sunar. Bir modül devre dışı bırakmak için basit ve hızlı yolu budur. Örneğin, HTTP yeniden yönlendirme modülü ile devre dışı bırakılabilir  **\<httpRedirect >** öğesinde *web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Yapılandırma ayarları modüllerle devre dışı bırakma hakkında daha fazla bilgi için bağlantıları izleyin *alt öğelerini* bölümünü [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Modül kaldırma

Bir ayar modülü kaldırmak için kullanmama varsa *web.config*, modülün kilidini açmak ve kilidini  **\<modülleri >** bölümünü *web.config* ilk:

1. Sunucu düzeyinde modülü kilidini açın. IIS Yöneticisi'nde IIS sunucusunu seçin **bağlantıları** kenar. Açık **modülleri** içinde **IIS** alanı. Modül listesinde seçin. İçinde **Eylemler** sağ kenar seçin **Unlock**. Kaldırmak planlama yaparken gibi birçok modül kilidini *web.config* daha sonra.

2. Uygulama olmadan dağıtmak bir  **\<modülleri >** bölümüne *web.config*. Bir uygulama ile dağıtılırsa bir *web.config* içeren  **\<modülleri >** bölüm önce Configuration Manager IIS Yöneticisi'nde kilidi olmadan bölüm bir özel durum oluşturur bölümün kilidini açma girişiminde bulunulduğunda. Bu nedenle, uygulamayı olmadan dağıtmak istediğiniz bir  **\<modülleri >** bölümü.

3. Kilidini  **\<modülleri >** bölümünü *web.config*. İçinde **bağlantıları** kenar, Web sitesi seçin **siteleri**. İçinde **Yönetim** alanında, açık **yapılandırma Düzenleyicisi**. Gezinti denetimlerinin seçmek için kullanın `system.webServer/modules` bölümü. İçinde **Eylemler** sağ kenar Seç **Unlock** bölümü.

4. Bu noktada, bir  **\<modülleri >** bölüm eklenebilir *web.config* ile dosya bir  **\<kaldırma >** öğesi modülünü kaldırmak için uygulama. Birden çok  **\<kaldırma >** öğeleri, birden fazla modülü kaldırmak için eklenebilir. Varsa *web.config* sunucu üzerinde değişiklik yapıldıysa, hemen aynı projenin değişiklik *web.config* yerel olarak dosya. Bu şekilde bir modül kaldırma, sunucudaki diğer uygulamalarla modülü kullanımını etkilemez.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Bir IIS modülü ile de kaldırılabilir *Appcmd.exe*. Sağlamak `MODULE_NAME` ve `APPLICATION_NAME` komutta:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Örneğin, kaldırma `DynamicCompressionModule` varsayılan Web sitesinden:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>En az modül yapılandırma

Bir ASP.NET Core uygulamayı çalıştırmak için gerekli yalnızca Anonim kimlik doğrulama modülünü ve ASP.NET Core modülü modüllerdir.

![IIS Yöneticisi'ni açmak için modüller gösterilen en az modül yapılandırmasıyla](modules/_static/modules.png)

URI önbelleği Modülü (`UriCacheModule`) IIS önbelleği Web sitesi yapılandırması URL düzeyinde izin verir. Bu modül IIS okumak ve hatta aynı URL'ye art arda istendiğinde yapılandırma her istekte ayrıştırmak gerekir. Yapılandırma ayrıştırılırken her isteği önemli bir performans sorunu sonuçlanır. *URI önbelleği modülü çalıştırmak barındırılan bir ASP.NET Core uygulama için kesinlikle gerekli olmasa da, tüm ASP.NET Core dağıtımları için URI önbelleğe alma modülü etkinleştirilmesi önerilir.*

Önbelleğe alma HTTP modülü (`HttpCacheModule`) IIS çıktı önbelleği ve ayrıca HTTP.sys önbelleğindeki öğeler önbelleğe alma için mantığı uygular. Bu modül olmadan içeriği artık çekirdek modunda önbelleğe alınır ve önbellek profilleri göz ardı edilir. Önbelleğe alma HTTP modülü genellikle kaldırma performans ve kaynak kullanımı üzerinde olumsuz etkilere sahiptir. *Önbelleğe alma HTTP modülü çalıştırmak barındırılan bir ASP.NET Core uygulama için kesinlikle gerekli olmasa da, tüm ASP.NET Core dağıtımları için önbelleğe alma HTTP modülü etkin olmasını öneririz.*

## <a name="additional-resources"></a>Ek kaynaklar

* [IIS ile Windows’da barındırma](xref:host-and-deploy/iis/index)
* [IIS mimarileri giriş: IIS modülleri](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS modülleri genel bakış](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [IIS 7.0 rolleri ve modülleri özelleştirme](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
