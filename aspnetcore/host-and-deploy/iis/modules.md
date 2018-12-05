---
title: ASP.NET Core içeren IIS modülleri
author: guardrex
description: ASP.NET Core uygulamaları ve IIS modüllerini yönetmek nasıl etkin ve etkin olmayan IIS modülleri keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/modules
ms.openlocfilehash: c6a6cc9b6b3410267c6f5034f824648a1ebbe10f
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862245"
---
# <a name="iis-modules-with-aspnet-core"></a>ASP.NET Core içeren IIS modülleri

Tarafından [Luke Latham](https://github.com/guardrex)

Bazı yerel IIS modülleri ve tüm yönetilen IIS modüllerini ASP.NET Core uygulamaları için istekleri işlemesini mümkün değildir. Çoğu durumda, ASP.NET Core IIS yerel ve yönetilen modülleri tarafından ele alınan senaryolar için bir alternatif sunar.

## <a name="native-modules"></a>Yerel modülleri

ASP.NET Core uygulamaları ile işlevsel yerel IIS modülleri ve ASP.NET Core modülü tabloyu belirtir.

| Modül | ASP.NET Core uygulamaları ile işlev | ASP.NET Core seçeneği |
| --- | :---: | --- |
| **Anonim kimlik doğrulaması**<br>`AnonymousAuthenticationModule`                                  | Evet | |
| **Temel Kimlik Doğrulaması**<br>`BasicAuthenticationModule`                                          | Evet | |
| **İstemci sertifika eşlemesi kimlik doğrulaması**<br>`CertificateMappingAuthenticationModule`      | Evet | |
| **CGI**<br>`CgiModule`                                                                           | Hayır  | |
| **Yapılandırmayı doğrulama**<br>`ConfigurationValidationModule`                                  | Evet | |
| **HTTP Hataları**<br>`CustomErrorModule`                                                           | Hayır  | [Durum kodu sayfa ara yazılımı](xref:fundamentals/error-handling#configure-status-code-pages) |
| **Özel günlük**<br>`CustomLoggingModule`                                                      | Evet | |
| **Varsayılan Belge**<br>`DefaultDocumentModule`                                                  | Hayır  | [Varsayılan dosya ara yazılımı](xref:fundamentals/static-files#serve-a-default-document) |
| **Özet kimlik doğrulaması**<br>`DigestAuthenticationModule`                                        | Evet | |
| **Dizin Tarama**<br>`DirectoryListingModule`                                               | Hayır  | [Dizin tarama ara yazılımı](xref:fundamentals/static-files#enable-directory-browsing) |
| **Dinamik sıkıştırma**<br>`DynamicCompressionModule`                                            | Evet | [Yanıt Sıkıştırma Ara Yazılımı](xref:performance/response-compression) |
| **İzleme**<br>`FailedRequestsTracingModule`                                                     | Evet | [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index#tracesource-provider) |
| **Dosyayı önbelleğe alma**<br>`FileCacheModule`                                                            | Hayır  | [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware) |
| **HTTP önbelleğe alma**<br>`HttpCacheModule`                                                            | Hayır  | [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware) |
| **HTTP Günlüğü**<br>`HttpLoggingModule`                                                          | Evet | [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index) |
| **HTTP Yeniden Yönlendirmesi**<br>`HttpRedirectionModule`                                                  | Evet | [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) |
| **IIS istemci sertifika eşlemesi kimlik doğrulaması**<br>`IISCertificateMappingAuthenticationModule` | Evet | |
| **IP ve Etki Alanı Kısıtlamaları**<br>`IpRestrictionModule`                                          | Evet | |
| **ISAPI Filtreleri**<br>`IsapiFilterModule`                                                         | Evet | [Ara Yazılım](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule`                                                                       | Evet | [Ara Yazılım](xref:fundamentals/middleware/index) |
| **Protokol desteği**<br>`ProtocolSupportModule`                                                  | Evet | |
| **İstek Filtreleme**<br>`RequestFilteringModule`                                                | Evet | [URL yeniden yazma ara yazılımı `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **İstek İzleyicisi**<br>`RequestMonitorModule`                                                    | Evet | |
| **URL yeniden yazma**&#8224;<br>`RewriteModule`                                                      | Evet | [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) |
| **Sunucu Tarafı İçermeler**<br>`ServerSideIncludeModule`                                            | Hayır  | |
| **Statik sıkıştırma**<br>`StaticCompressionModule`                                              | Hayır  | [Yanıt Sıkıştırma Ara Yazılımı](xref:performance/response-compression) |
| **Statik İçerik**<br>`StaticFileModule`                                                         | Hayır  | [Statik dosya ara yazılımlarını](xref:fundamentals/static-files) |
| **Belirteç önbelleğe alma**<br>`TokenCacheModule`                                                          | Evet | |
| **URI önbelleği**<br>`UriCacheModule`                                                              | Evet | |
| **URL Yetkilendirmesi**<br>`UrlAuthorizationModule`                                                | Evet | [ASP.NET Core kimliği](xref:security/authentication/identity) |
| **Windows kimlik doğrulaması**<br>`WindowsAuthenticationModule`                                      | Evet | |

&#8224;URL yeniden yazma modülü 's `isFile` ve `isDirectory` eşleşme türleri ile ASP.NET Core uygulamaları yapılan değişiklikler nedeniyle çalışmıyor [dizin yapısı](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Yönetilen modüller

Yönetilen modüller *değil* uygulama havuzunun .NET CLR sürümü ayarlandığında barındırılan ASP.NET Core uygulamaları ile işlevsel **yönetilen kod yok**. ASP.NET Core, bazı durumlarda ara yazılımı seçenekleri sunar.

| Modül                  | ASP.NET Core seçeneği |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Tanımlama bilgisi kimlik doğrulaması ara yazılımı](xref:security/authentication/cookie) |
| outputCache             | [Yanıtları Önbelleğe Alma Ara Yazılımı](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Oturum                 | [Oturum ara yazılımı](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [URL Yeniden Yazma Ara Yazılımı](xref:fundamentals/url-rewriting) |
| UrlRoutingModule 4.0    | [ASP.NET Core kimliği](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>IIS Yöneticisi'ni uygulama değişiklikleri

Ayarları yapılandırmak için IIS Yöneticisi'ni kullanırken *web.config* uygulamanın dosya değiştirilir. Uygulama dağıtımı ve dahil olmak üzere *web.config*, IIS Yöneticisi ile yapılan değişikliklerin üzerine tarafından dağıtılan *web.config* dosya. Sunucunun değişiklikler yaptıysanız *web.config* dosya, güncelleştirilmiş Kopyala *web.config* hemen yerel proje sunucudaki dosya.

## <a name="disabling-iis-modules"></a>IIS modüllerini devre dışı bırakma

Ek olarak, uygulamanın bir uygulama için devre dışı bırakılması gereken sunucu düzeyinde bir IIS Modülü yapılandırılıp yapılandırılmadığını *web.config* dosya modül devre dışı bırakabilirsiniz. Modül yerinde bırakın ve bir yapılandırma ayarı (varsa) kullanarak devre dışı bırakın veya modül uygulamadan kaldırın.

### <a name="module-deactivation"></a>Modül devre dışı bırakma

Fazla modül, modül uygulamadan kaldırmadan devre dışı bırakılmasına olanak veren bir yapılandırma ayarı sağlar. Bir modül devre dışı bırakmak için basit ve hızlı bir yolu budur. Örneğin, HTTP yeniden yönlendirme modülü ile devre dışı bırakılabilir `<httpRedirect>` öğesinde *web.config*:

```xml
<configuration>
  <system.webServer>
    <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Yapılandırma ayarlarıyla modülleri devre dışı bırakma hakkında daha fazla bilgi için bağlantıları izleyin *alt öğeleri* bölümünü [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Modül kaldırma

Bir ayar modülü kaldırmak için kabul edilirse, *web.config*, modülün kilidini açmak ve kilidini `<modules>` bölümünü *web.config* ilk:

1. Sunucu düzeyinde modülün kilidini açın. IIS Yöneticisi'nde IIS sunucusu **bağlantıları** kenar çubuğu. Açık **modülleri** içinde **IIS** alan. Modül listesinde seçin. İçinde **eylemleri** sağ kenar seçin **kilidini**. Kilidini kaldırmak planlarken kadar modülleri *web.config* daha sonra.

2. Gerekmeden uygulamayı dağıtma bir `<modules>` konusundaki *web.config*. Bir uygulama ile dağıtılırsa bir *web.config* içeren `<modules>` bölüm bölüm Configuration Manager IIS Yöneticisi'nde ilk kilidi kalmadan, bölümün kilidini açmak çalışırken bir özel durum oluşturur. Bu nedenle, gerekmeden uygulamayı dağıtma bir `<modules>` bölümü.

3. Kilit açma `<modules>` bölümünü *web.config*. İçinde **bağlantıları** kenar, Web sitesi seçin **siteleri**. İçinde **Yönetim** alanında açık **yapılandırma Düzenleyicisi**. Gezinti denetimlerinin seçmek için kullanın `system.webServer/modules` bölümü. İçinde **eylemleri** sağ kenar Seç **kilidini** bölümü.

4. Bu noktada, bir `<modules>` bölüm eklenebilir *web.config* ile dosya bir `<remove>` uygulamadan modülünü kaldırmak için öğesi. Birden çok `<remove>` öğeleri birden çok modül kaldırmak için eklenebilir. Varsa *web.config* sunucu üzerinde yapılan değişiklikler, hemen aynı projenin değişiklik *web.config* dosyasını yerel. Bu şekilde bir modül kaldırma, sunucudaki diğer uygulamalarla modülü kullanımını etkilemez.

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

ASP.NET Core uygulaması çalıştırmak için gereken yalnızca Anonim kimlik doğrulama modülünü ve ASP.NET Core modülü modüllerdir.

URI önbelleği Modülü (`UriCacheModule`) IIS URL düzeyinde önbellek Web sitesi yapılandırmasını sağlar. Bu modül IIS okuyun ve hatta aynı URL'yi art arda istendiğinde her istekte yapılandırmasını ayrıştırmamasıyla gerekir. Yapılandırma ayrıştırılırken her istek bir önemli bir performans düşüşüyle sonuçlanır. *URI önbelleği modülü çalıştırmak barındırılan bir ASP.NET Core uygulaması için kesinlikle gerekli olmasa da, tüm ASP.NET Core dağıtımları için URI önbelleğe alma modülü etkin olmasını öneririz.*

HTTP önbelleğe alma Modülü (`HttpCacheModule`) IIS çıktı önbelleğini ve ayrıca HTTP.sys önbelleğini öğeleri önbelleğe alma mantığını uygular. Bu modül olmadan içeriği artık çekirdek modunda önbelleğe alınır ve önbellek profilleri göz ardı edilir. HTTP önbelleğe alma modülü genellikle kaldırma, performans ve kaynak kullanımını olumsuz etkileri vardır. *HTTP önbelleğe alma modülü çalıştırmak barındırılan bir ASP.NET Core uygulaması için kesinlikle gerekli olmasa da, tüm ASP.NET Core dağıtımları için önbelleğe alma HTTP modülü etkin olmasını öneririz.*

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/iis/index>
* [IIS mimarileri giriş: IIS modülleri](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [IIS modülleri genel bakış](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [IIS 7.0 rolleri ve modülleri özelleştirme](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
