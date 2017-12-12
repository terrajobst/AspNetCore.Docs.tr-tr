---
title: "ASP.NET Core ile IIS modüllerini kullanma"
author: guardrex
description: "ASP.NET Core uygulamaları için etkin ve etkin olmayan IIS modüllerini açıklayan başvuru belgesi."
keywords: "ASP.NET Core, IIS, modül, ters proxy"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: fee8e830ab43f731de9c90fad06b577662760f87
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>ASP.NET çekirdeği ile IIS modüllerini kullanma

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamaları IIS tarafından bir ters proxy yapılandırması barındırılır. Yerel IIS modülleri bazıları ve tüm yönetilen IIS modülleri ASP.NET Core uygulamaları için istekleri işlemesini kullanılabilir değil. Çoğu durumda, ASP.NET Core IIS yerel ve yönetilen modülleri özelliklerine bir alternatif sunar.

## <a name="native-modules"></a>Yerel modülleri
Modül | .NET core etkin | ASP.NET çekirdeği seçeneği
--- | :---: | ---
**Anonim kimlik doğrulaması**<br>`AnonymousAuthenticationModule` | Evet | 
**Temel Kimlik Doğrulaması**<br>`BasicAuthenticationModule` | Evet | 
**İstemci sertifika eşlemesi kimlik doğrulaması**<br>`CertificateMappingAuthenticationModule` | Evet | 
**CGI**<br>`CgiModule` | Hayır | 
**Yapılandırma Doğrulama**<br>`ConfigurationValidationModule` | Evet | 
**HTTP Hataları**<br>`CustomErrorModule` | Hayır | [Durum kodu sayfaları Ara](xref:fundamentals/error-handling#configuring-status-code-pages)
**Özel günlüğe kaydetme**<br>`CustomLoggingModule` | Evet | 
**Varsayılan Belge**<br>`DefaultDocumentModule` | Hayır | [Varsayılan dosya ara yazılımı](xref:fundamentals/static-files#serving-a-default-document)
**Özet kimlik doğrulaması**<br>`DigestAuthenticationModule` | Evet | 
**Dizin Tarama**<br>`DirectoryListingModule` | Hayır | [Dizin tarama Ara](xref:fundamentals/static-files#enabling-directory-browsing)
**Dinamik sıkıştırma**<br>`DynamicCompressionModule` | Evet | [Yanıt sıkıştırma Ara](xref:performance/response-compression)
**İzleme**<br>`FailedRequestsTracingModule` | Evet | [ASP.NET çekirdeği günlüğü](xref:fundamentals/logging/index#the-tracesource-provider)
**Dosyayı önbelleğe alma**<br>`FileCacheModule` | Hayır | [Yanıt önbelleğe alma Ara](xref:performance/caching/middleware)
**HTTP önbelleğe alma**<br>`HttpCacheModule` | Hayır | [Yanıt önbelleğe alma Ara](xref:performance/caching/middleware)
**HTTP Günlüğü**<br>`HttpLoggingModule` | Evet | [ASP.NET çekirdeği günlüğü](xref:fundamentals/logging/index)<br>Uygulamaları: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**HTTP Yeniden Yönlendirmesi**<br>`HttpRedirectionModule` | Evet | [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting)
**IIS istemci sertifika eşlemesi kimlik doğrulaması**<br>`IISCertificateMappingAuthenticationModule` | Evet | 
**IP ve Etki Alanı Kısıtlamaları**<br>`IpRestrictionModule` | Evet | 
**ISAPI Filtreleri**<br>`IsapiFilterModule` | Evet | [Ara yazılım](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Evet | [Ara yazılım](xref:fundamentals/middleware)
**Protokol desteği**<br>`ProtocolSupportModule` | Evet | 
**İstek Filtreleme**<br>`RequestFilteringModule` | Evet | [URL yeniden yazma işlemi Ara`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**İstek İzleyicisi**<br>`RequestMonitorModule` | Evet | 
**URL yeniden yazma işlemi**<br>`RewriteModule` | Yes† | [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting)
**Sunucu Tarafına Şunlar Dahildir**<br>`ServerSideIncludeModule` | Hayır | 
**Statik sıkıştırma**<br>`StaticCompressionModule` | Hayır | [Yanıt sıkıştırma Ara](xref:performance/response-compression)
**Statik İçerik**<br>`StaticFileModule` | Hayır | [Statik dosya ara yazılımı](xref:fundamentals/static-files)
**Belirteç önbelleğe alma**<br>`TokenCacheModule` | Evet | 
**URI önbelleği**<br>`UriCacheModule` | Evet | 
**URL Yetkilendirmesi**<br>`UrlAuthorizationModule` | Evet | [ASP.NET Core kimliği](xref:security/authentication/identity)
**Windows kimlik doğrulaması**<br>`WindowsAuthenticationModule` | Evet | 

†The URL yeniden yazma modülü 's `isFile` ve `isDirectory` değişiklikleri nedeniyle ASP.NET Core uygulamaları ile çalışmaz [dizin yapısını](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Yönetilen modüller
Modül | .NET core etkin | ASP.NET çekirdeği seçeneği
--- | :---: | ---
AnonymousIdentification | Hayır | 
DefaultAuthentication | Hayır | 
FileAuthorization | Hayır | 
FormsAuthentication | Hayır | [Tanımlama bilgisi kimlik doğrulaması ara yazılımı](xref:security/authentication/cookie)
OutputCache | Hayır | [Yanıt önbelleğe alma Ara](xref:performance/caching/middleware)
Profil | Hayır | 
RoleManager | Hayır | 
ScriptModule 4.0 | Hayır | 
Oturum | Hayır | [Oturum Ara](xref:fundamentals/app-state)
UrlAuthorization | Hayır | 
UrlMappingsModule | Hayır | [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | Hayır | [ASP.NET Core kimliği](xref:security/authentication/identity)
WindowsAuthentication | Hayır | 

## <a name="iis-manager-application-changes"></a>IIS Yöneticisi'ni uygulama değişiklikleri
Ayarlarını yapılandırmak için IIS Yöneticisi'ni kullandığınızda, doğrudan değiştirmekte *web.config* uygulamanın dosya. Uygulamanızı dağıtma ve dahil *web.config*, IIS Yöneticisi ile yapılan değişikliklerin üzerine yazılır tarafından dağıtılan *web.config dosyasında*. Bu nedenle sunucunun değişiklik yaparsanız *web.config* dosya, güncelleştirilmiş Kopyala *web.config* hemen yerel projeniz dosya.

## <a name="disabling-iis-modules"></a>IIS modülleri devre dışı bırakma
Bir uygulama için devre dışı bırakmak istediğiniz sunucu düzeyinde yapılandırılan bir IIS Modülü varsa, ek olarak ile bunu yapabilirsiniz, *web.config* dosya. Modül bırakın ve (varsa) bir yapılandırma ayarını kullanarak devre dışı bırakın ya da uygulamadan modülünü Kaldır.

### <a name="module-deactivation"></a>Modül devre dışı bırakma
Birçok modül uygulamadan kaldırmadan devre dışı bırakmanızı sağlayacak bir yapılandırma ayarı sunar. Bir modül devre dışı bırakmak için basit ve hızlı yolu budur. IIS URL yeniden yazma modülü devre dışı bırakmak istiyorsanız kullanın örneğin `<httpRedirect>` aşağıda gösterildiği gibi öğesi. Yapılandırma ayarları modüllerle devre dışı bırakma hakkında daha fazla bilgi için bağlantıları izleyin *alt öğelerini* bölümünü [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Modül kaldırma
Bir ayar modülü kaldırmak için kabul *web.config*, modülün kilidini açmak ve kilidini `<modules>` bölümünü *web.config* ilk. Adımlar aşağıda özetlenen:

1. Sunucu düzeyinde modülü kilidini açın. ' I tıklatın IIS sunucusunda IIS Yöneticisi'nde **bağlantıları** kenar. Açık **modülleri** içinde **IIS** alanı. Modül listesinde tıklayın. İçinde **Eylemler** Kenar çubuğunda sağ tıklatın **Unlock**. İle kaldırmak planlama yaparken gibi birçok modül kilidini *web.config* daha sonra.

2. Uygulamanızı olmadan dağıtmak bir `<modules>` bölümüne *web.config*. Bir uygulamayı dağıtırsanız bir *web.config* içeren `<modules>` bölüm önce Configuration Manager IIS Yöneticisi'nde kilidi olmadan bölüm bölümün kilidini açmayı denediğinizde bir özel durum oluşturur. Bu nedenle, uygulamanızı olmadan dağıtmak bir `<modules>` bölümü.

3. Kilidini `<modules>` bölümünü *web.config*. İçinde **bağlantıları** kenar, Web sitesi tıklatın **siteleri**. İçinde **Yönetim** alanında, açık **yapılandırma Düzenleyicisi**. Gezinti denetimlerinin seçmek için kullanın `system.webServer/modules` bölümü. İçinde **Eylemler** Kenar çubuğunda sağ tıklatıp **Unlock** bölümü.

4. Bu noktada, eklemeniz mümkün olacaktır bir `<modules>` için bölüm, *web.config* ile dosya bir `<remove>` öğesi uygulamadan modülünü Kaldır. Birden çok ekleyebilirsiniz `<remove>` öğeleri birden fazla modülü kaldırmak için. Yaptığınız unutmayın *web.config* hemen projeyi yerel olarak yapmanız için sunucu üzerindeki değişiklikler. Bu şekilde bir modül kaldırma, sunucudaki diğer uygulamalarla modülü kullanımınız etkilemez.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Yüklü varsayılan modüllerle bir IIS yüklemesi için aşağıdakileri kullanabilirsiniz `<module>` varsayılan modülleri kaldırmak için bölümü.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

Bir IIS modülüyle da kaldırabilirsiniz *Appcmd.exe*. Sağlamak `MODULE_NAME` ve `APPLICATION_NAME` aşağıda gösterilen komuttaki:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Nasıl kaldırılacağı işte `DynamicCompressionModule` varsayılan Web sitesinden:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>En az modül yapılandırması
Bir ASP.NET Core uygulamayı çalıştırmak için gerekli yalnızca Anonim kimlik doğrulama modülünü ve ASP.NET Core modülü modüllerdir.

![IIS Yöneticisi'ni açmak için modüller gösterilen en az modül yapılandırmasıyla](iis-modules/_static/modules.png)

## <a name="resources"></a>Kaynaklar
* [IIS yayımlama](xref:publishing/iis)
* [IIS modülleri genel bakış](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [IIS 7.0 rolleri ve modülleri özelleştirme](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
