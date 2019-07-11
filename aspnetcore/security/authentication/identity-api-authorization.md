---
title: ASP.NET core'da tek sayfalı uygulamalar için kimlik doğrulamaya giriş
author: javiercn
description: ASP.NET Core uygulaması içinde barındırılan bir tek sayfalı uygulama kimliğiyle kullanın.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 302a5e10a70e40e75ab9fe4b3e5a98c4e847b822
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815227"
---
# <a name="authentication-and-authorization-for-spas"></a>Kimlik doğrulama ve yetkilendirme Spa'lar için

ASP.NET Core 3.0 veya üstü API yetkilendirme için destek kullanarak tek sayfa uygulamaları (Spa'lar) kimlik doğrulaması sunar. Kimlik doğrulama ve kullanıcıları depolamak için ASP.NET Core kimliği ile birlikte [IdentityServer](https://identityserver.io/) Open ID Connect uygulamak için.

Authentication parametresi eklendi **Angular** ve **React** proje kimlik doğrulaması parametresinde benzer şablonları **Web uygulaması (Model-View-Controller)**  (MVC) ve **Web uygulaması** (Razor sayfaları) proje şablonları. İzin verilen parametre değerleri **hiçbiri** ve **bireysel**. **React.js ve Redux** proje şablonu, kimlik doğrulaması parametresi şu anda desteklemiyor.

## <a name="create-an-app-with-api-authorization-support"></a>API yetkilendirme desteği ile uygulama oluşturma

Kullanıcı kimlik doğrulaması ve yetkilendirme, Angular ve Spa'lar React ile kullanılabilir. Bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:

**Angular**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**React**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

Önceki komutu ile bir ASP.NET Core uygulaması oluşturur. bir *ClientApp* SPA içeren dizin.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>ASP.NET Core bileşenlerinin uygulamasının genel açıklaması

Aşağıdaki bölümlerde, kimlik doğrulama desteği dahil edildiğinde projeye eklemeler açıklanmaktadır:

### <a name="startup-class"></a>Başlangıç sınıfı

`Startup` Sınıfı aşağıdaki eklemelerle sahiptir:

* İçinde `Startup.ConfigureServices` yöntemi:
  * Kimliği ' % s'varsayılan kullanıcı Arabirimi ile:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * Bir ek IdentityServer `AddApiAuthorization` yardımcı yöntemi bu ayarları bazı varsayılan IdentityServer üzerinde ASP.NET Core kuralları:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Kimlik doğrulaması ile ek `AddIdentityServerJwt` JWT belirteçleri doğrulamak üzere uygulamayı yapılandırır yardımcı yöntemi tarafından IdentityServer üretilen:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* İçinde `Startup.Configure` yöntemi:
  * İstek kimlik doğrulama ve kullanıcı istek içeriğine ayarını sorumludur kimlik doğrulaması ara yazılımı:

    ```csharp
    app.UseAuthentication();
    ```

  * Open ID Connect uç noktayı kullanıma sokan IdentityServer ara:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Bu yardımcı yöntem IdentityServer bizim desteklenen yapılandırmasını kullanmak üzere yapılandırır. IdentityServer uygulama güvenlik kaygıları işleme için güçlü ve Genişletilebilir bir çerçevedir. Aynı anda en yaygın senaryolar için gereksiz karmaşıklık kullanıma sunar. Sonuç olarak, birtakım kuralları ve yapılandırma seçenekleri, iyi bir başlangıç noktası olarak kabul edilir sağlanır. Kimlik Doğrulamanızın ihtiyaçları değiştiğinde sonra IdentityServer'ın kimlik doğrulaması gereksinimlerinize göre özelleştirmek yine de kullanılabilir.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Bu yardımcı yöntemi, uygulama için bir ilke düzeni varsayılan kimlik doğrulaması işleyici yapılandırır. İlke kimlik herhangi bir alt kimlik URL'si alanında yönlendirilen tüm istekleri işlemeye izin vermek için yapılandırılmış "/ kimlik". `JwtBearerHandler` Diğer tüm istekleri işler. Ayrıca, bu yöntem kaydeder bir `<<ApplicationName>>API` IdentityServer API kaynakla varsayılan kapsamı ile `<<ApplicationName>>API` ve uygulama için IdentityServer tarafından verilen belirteçleri doğrulamak için JWT taşıyıcı belirteç ara yazılımı yapılandırır.

### <a name="sampledatacontroller"></a>SampleDataController

İçinde *Controllers\SampleDataController.cs* dosya, fark `[Authorize]` kullanıcı yetki verilmesi gerektiğini belirten sınıfına uygulanan bir öznitelik tabanlı kaynağa erişmek için varsayılan ilke. Varsayılan kimlik doğrulama şeması tarafından ayarlanan kullanacak şekilde yapılandırılması için varsayılan yetkilendirme ilkesi olur `AddIdentityServerJwt` yukarıda, yapmadan bahsedilen ilke şemasına `JwtBearerHandler` gibi yardımcı yöntemi tarafından yapılandırılan varsayılan işleyicisi Uygulama istekleri.

### <a name="applicationdbcontext"></a>ApplicationDbContext

İçinde *Data\ApplicationDbContext.cs* dosya, aynı dikkat edin `DbContext` kimliğinde bunu genişleten bir özel durum ile kullanılan `ApiAuthorizationDbContext` (daha türetilmiş sınıftan `IdentityDbContext`) IdentityServer için şemayı içerecek şekilde.

Veritabanı şeması tam olarak denetlemek için kullanılabilen kimlik birinden devralan `DbContext` sınıfları ve bağlam çağırarak kimlik şemayı içerecek şekilde yapılandırmak `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` üzerinde `OnModelCreating` yöntemi.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

İçinde *Controllers\OidcConfigurationController.cs* dosya, sağlanan istemci kullanması gereken OIDC parametreleri hizmet için uç nokta dikkat edin.

### <a name="appsettingsjson"></a>appsettings.json

İçinde *appsettings.json* dosya proje kökündeki var. yeni bir `IdentityServer` listesini açıklayan bölümünde yapılandırılan istemciler. Aşağıdaki örnekte, tek bir istemci yoktur. İstemci adı uygulama adına karşılık gelir ve OAuth için kural tarafından eşleştirilen `ClientId` parametresi. Profil, yapılandırılan uygulama türünü belirtir. Ayrıca, sunucu için yapılandırma işlemini basitleştirmek sürücü kuralları için dahili olarak kullanılır. Kullanılabilir birkaç profil, açıklandığı şekilde [uygulama profilleri](#application-profiles) bölümü.

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appSettings. Development.JSON

İçinde *appsettings. Development.JSON* proje kök dosya var. bir `IdentityServer` Belirteçleri imzalamak için kullanılan anahtarı açıklayan bölümü. Üretime dağıtırken, bir anahtarın sağlanabilir ve uygulama ile birlikte dağıtılan açıklandığı şekilde gerekir [üretime dağıtma](#deploy-to-production) bölümü.

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>Angular uygulamanın genel açıklaması

Angular içinde API yetkilendirme ve kimlik doğrulama desteği şablon bulunduğu kendi Angular modülünde *ClientApp\src\api yetkilendirme* dizin. Modül aşağıdaki öğelerden oluşur:

* 3 bileşenler:
  * *Login.Component.TS*: Uygulamanın oturum açma akışını yönetir.
  * *Logout.Component.TS*: Uygulamanın oturum kapatma akış işler.
  * *oturum açma menu.component.ts*: Aşağıdaki adımlardan birini bağlantılarını görüntüleyen bir pencere öğesi:
    * Kullanıcı profili yönetimi ve oturumu kapatma kullanıcının kimliği doğrulandığında bağlantıları.
    * Kayıt ve oturum açma kullanıcı kimliği değil, bağlantıları.
* Bir rota guard `AuthorizeGuard` yolları eklenebilir ve rota gitmeden önce doğrulanmasını gerektirir.
* Bir HTTP dinleyiciyi `AuthorizeInterceptor` kullanıcının kimliği doğrulandığında, API'yi hedefleyen giden HTTP isteklerini erişim belirtecini ekler.
* Bir hizmet `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için tüketim için uygulama kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.
* Uygulamanın kimlik doğrulaması bölümleriyle ilgili yolları tanımlar Angular modülü. Oturum açma menü bileşeni, dinleyiciyi, koruma ve uygulama geri kalanından hizmetini kullanıma sunar.

## <a name="general-description-of-the-react-app"></a>React uygulamanın genel açıklaması

Kimlik doğrulama ve yetkilendirme React şablonu API desteği bulunan *ClientApp\src\components\api yetkilendirme* dizin. Bunu aşağıdaki öğelerden oluşur:

* 4 bileşenler:
  * *Login.js*: Uygulamanın oturum açma akışını yönetir.
  * *Logout.js*: Uygulamanın oturum kapatma akış işler.
  * *LoginMenu.js*: Aşağıdaki adımlardan birini bağlantılarını görüntüleyen bir pencere öğesi:
    * Kullanıcı profili yönetimi ve oturumu kapatma kullanıcının kimliği doğrulandığında bağlantıları.
    * Kayıt ve oturum açma kullanıcı kimliği değil, bağlantıları.
  * *AuthorizeRoute.js*: Belirtilen bileşen işlemeden önce doğrulanması bir kullanıcı gerektiren bir yol bileşeni `Component` parametresi.
* Dışarı aktarılan bir `authService` sınıfının örneğini `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için tüketim için uygulama kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.

Çözüm ana bileşenleri gördüğünüze göre uygulama için bireysel senaryolar daha ayrıntılı bir göz atabilirsiniz.

## <a name="require-authorization-on-a-new-api"></a>Yeni bir API üzerinde Yetkilendirme gerektirir

Varsayılan olarak, sistem, yetkilendirme için yeni API'leri kolayca gerektirecek şekilde yapılandırılır. Bunu yapmak için yeni bir denetleyici oluşturma ve ekleme `[Authorize]` denetleyicideki tüm eylem veya denetleyici sınıfı özniteliği.

## <a name="protect-a-client-side-route-angular"></a>Bir istemci-tarafı yol (Angular) koruma

Bir istemci-tarafı yol koruma authorize guard bir rota yapılandırırken çalıştırılacak cf listesine ekleyerek gerçekleştirilir. Örneğin, gördüğünüz nasıl `fetch-data` rota ana uygulama Angular modülü içinde yapılandırılır:

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Bir rota koruma gerçek bir uç nokta koruma sağlamaz bahsetmek önemlidir (yine de gerektiren bir `[Authorize]` özniteliğinin) doğrulaması yapılmaz, belirli bir istemci-tarafı rota gezinme yalnızca kullanıcının engeller ancak bu.

## <a name="authenticate-api-requests-angular"></a>API isteklerinin (Angular) kimlik doğrulaması

Uygulamanın yanı sıra barındırılan API'lere isteklerinde kimlik doğrulama kullanarak uygulama tarafından tanımlanmış HTTP istemci dinleyiciyi otomatik olarak yapılır.

## <a name="protect-a-client-side-route-react"></a>Bir istemci-tarafı yol (React) koruma

Bir istemci-tarafı rota korur `AuthorizeRoute` düz yerine bileşen `Route` bileşeni. Örneğin, fark nasıl `fetch-data` yol içinde yapılandırılmış `App` bileşeni:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Bir rota koruma:

* Gerçek bir uç nokta koruma değil (yine de gerektiren bir `[Authorize]` özniteliğinin).
* Yalnızca kullanıcı doğrulaması yapılmaz, belirli bir istemci-tarafı rota gitmesini engeller.

## <a name="authenticate-api-requests-react"></a>API isteklerinin (React) kimlik doğrulaması

React ile isteklerine kimlik doğrulaması yapılır ilk içeri aktararak `authService` gelen örnek `AuthorizeService`. Erişim belirteci alınır `authService` ve aşağıda gösterildiği gibi isteğe bağlı. React bileşenlerde bu iş genellikle yapılabilir `componentDidMount` yaşam döngüsü yöntemi veya kullanıcı etkileşimi sonucu olarak.

### <a name="import-the-authservice-into-your-component"></a>Bileşeniniz authService içe

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Alın ve yanıtı erişim belirteci ekleme

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a>Üretime dağıtma

Uygulamayı üretime dağıtmak için aşağıdaki kaynakları sağlanması gerekir:

* Kimlik kullanıcı hesapları ve IdentityServer depolamak için bir veritabanı verir.
* Belirteçleri imzalamak için kullanmak üzere bir üretim sertifika.
  * Bu sertifika için belirli gereksinimler yoktur; otomatik olarak imzalanan bir sertifika veya bir CA yetkilisi sağlanan bir sertifika olabilir.
  * PowerShell veya OpenSSL gibi standart araçlar aracılığıyla oluşturulabilir.
  * Hedef makinelerde sertifika deposuna yüklenmesi veya olarak dağıtılan bir *.pfx* güçlü bir parola ile dosya.

### <a name="example-deploy-to-azure-websites"></a>Örnek: Azure Web Siteleri'ne dağıtma

Bu bölümde, uygulama sertifika depolama alanında depolanan bir sertifika kullanarak Azure Web siteleri'ne dağıtma açıklanır. Sertifika deposundan bir sertifikayı yüklemek için uygulamayı değiştirmek için App Service planı, daha sonraki bir adımda yapılandırdığınızda üzerinde en az standart katmanı olması gerekir. Uygulamanın *appsettings.json* dosya, değişiklik `IdentityServer` bölümü önemli ayrıntıları içerir:

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* Sertifika adı özelliği ayırt edici konu sertifikasının karşılık gelir.
* Sertifikadan yükleneceği depolama konumu temsil eder (`CurrentUser` veya `LocalMachine`).
* Depo adını sertifikanın depolandığı sertifika deposunun adını temsil eder. Bu durumda, kullanıcı Kişisel depoya işaret eder.

Yer alan adımları uygulayarak bir uygulamayı Azure Web siteleri'ne dağıtma [uygulamasını Azure'a dağıtma](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) gerekli Azure kaynakları oluşturmak ve uygulamayı üretime dağıtın.

Yukarıdaki yönergeleri izleyerek sonra uygulamayı Azure'a dağıtılır, ancak henüz işlevsel değil. Uygulama tarafından kullanılan sertifikayı yine de ayarlanması gerekir. Kullanılacak sertifika parmak izini bulun ve açıklanan adımları [sertifikalarınızı yüklemek](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).

While bu adımları bahsetmek SSL, bir **özel sertifikaları** Portal karşıya yüklersiniz uygulamayla kullanmak için sağlanan sertifikanın bölümü.

Bu adımdan sonra uygulamayı yeniden başlatın ve işlev olmalıdır.

## <a name="other-configuration-options"></a>Diğer yapılandırma seçenekleri

API yetkilendirme için destek IdentityServer üzerine kuralları, varsayılan değerleri ve geliştirmeleri Spa'lar deneyimini basitleştirmek için bir dizi oluşturur. Deyin needless için ASP.NET Core tümleştirmeler senaryonuz erişmezse IdentityServer'ın arka planda kullanılabilir. ASP.NET Core desteği kuruluşumuz tarafından dağıtılan tüm uygulamalar burada oluşturulan ve "Birinci taraf" uygulamaları odaklanmıştır. Bu nedenle, destek, onay ve Federasyon gibi şeyler için sunulan değil. Bu senaryolar için IdentityServer kullanın ve kendi belgelerini izleyin.

### <a name="application-profiles"></a>Uygulama profilleri

Uygulama profilleri, kendi parametrelerini daha tanımlamanız uygulamalar için önceden tanımlanmış yapılandırmalardır. Şu anda aşağıdaki profilleri desteklenir:

* `IdentityServerSPA`: Tek bir birim olarak IdentityServer yanı sıra barındırılan bir SPA temsil eder.
  * `redirect_uri` Varsayılan olarak `/authentication/login-callback`.
  * `post_logout_redirect_uri` Varsayılan olarak `/authentication/logout-callback`.
  * Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.
  * İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).
  * İzin verilen yanıt modu `fragment`.
* `SPA`: Barındırılan değil bir SPA IdentityServer ile temsil eder.
  * Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.
  * İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).
  * İzin verilen yanıt modu `fragment`.
* `IdentityServerJwt`: Birlikte barındırılan bir API ile IdentityServer temsil eder.
  * Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.
* `API`: Barındırılan değil bir API ile IdentityServer temsil eder.
  * Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.

### <a name="configuration-through-appsettings"></a>AppSettings aracılığıyla yapılandırma

Yapılandırma sistemi aracılığıyla uygulamalar listesine ekleyerek yapılandırma `Clients` veya `Resources`.

Her istemcinin yapılandırma `redirect_uri` ve `post_logout_redirect_uri` aşağıdaki örnekte gösterildiği gibi özelliği:

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

Kaynakları yapılandırırken, kaynak için kapsamları aşağıda gösterildiği gibi yapılandırabilirsiniz:

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a>Kod ile yapılandırma

İstemciler ve kaynakları bir aşırı yüklemesini kullanarak kod aracılığıyla da yapılandırabilirsiniz `AddApiAuthorization` seçeneklerini yapılandırmak için bir eylemi alır.

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:spa/angular>
* <xref:spa/react>
