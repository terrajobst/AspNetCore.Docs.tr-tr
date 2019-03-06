---
title: ASP.NET core'da tek sayfalık uygulamalar için kimlik doğrulamaya giriş
author: ''
description: ASP.NET Core uygulaması içinde barındırılan bir tek sayfalı uygulama kimlik kullanın.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346820"
---
# <a name="authentication-and-authorization-for-spas"></a>Kimlik doğrulama ve yetkilendirme Spa'lar için

ASP.NET 3. 0'yeni desteğimiz için API yetkilendirme kullanarak tek sayfalı uygulama kimlik doğrulaması için destek sunuyoruz. Bu destek için kimlik doğrulama ve kullanıcıları ve kimlik sunucusu Open ID Connect uygulamak için depolama üzerinde ASP.NET Core kimliği birleşimini temel alır.

Yeni bir kimlik doğrulama parametresi için sunduğumuz Angular ekledik ve mvc ve razor sayfaları şablonlarımız ile kimlik doğrulaması parametresinde benzer React şablonları izin verilen değerler 'None' ve 'Bireysel'.

## <a name="create-an-angular-app-with-api-authorization-support"></a>API ile yetkilendirme desteği Angular uygulama oluşturma

Kimlik doğrulama ve kullanıcıları yetkilendirme desteğiyle yeni Angular uygulaması oluşturmak için bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

Önceki komutu ile bir ASP.NET Core uygulaması oluşturur. bir *ClientApp* Angular uygulamasını içeren dizin.

## <a name="create-a-react-app-with-api-authorization-support"></a>API yetkilendirme desteğiyle bir React uygulaması oluşturma

Kimlik doğrulama ve kullanıcıları yetkilendirme desteği ile yeni bir React uygulaması oluşturmak için bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:

```console
dotnet new react -o <output_directory_name> -au Individual
```

Önceki komutu ile bir ASP.NET Core uygulaması oluşturur. bir *ClientApp* React uygulamayı içeren dizin.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>ASP.NET Core bileşenlerinin uygulamasının genel açıklaması

Kimlik doğrulaması için destek ekliyoruz, birkaç proje eklemeler vardır:

### <a name="startup-class"></a>Başlangıç sınıfı

Başlangıç sınıfı aşağıdaki kodda baktığımızda aşağıdaki eklemeler veriyoruz:
* İçinde `public void ConfigureServices(IServiceCollection services)`:
  * Kimliği ' % s'varsayılan kullanıcı Arabirimi ile.
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * Kimlik sunucusu ile ek bir AddApiAuthorization yardımcı yöntem bu ayarları bazı varsayılan kimlik Server üzerinde ASP.NET kuralları.
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * Kimlik doğrulaması ile kimlik sunucusu tarafından üretilen Jwt belirteçleri doğrulamak üzere uygulamayı yapılandırır bir ek AddIdentityServerJwt yardımcı yöntemi. 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* İçinde `public void Configure(IApplicationBuilder app)`:
  * Gelen istek kimlik doğrulama ve kullanıcı istek içeriğine ayarını sorumludur kimlik doğrulaması ara yazılımı.
    ```csharp
    app.UseAuthentication();
    ```
  * Open ID Connect uç noktayı kullanıma sokan kimlik sunucusu ara yazılımı.
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
Bu yardımcı yöntem kimlik Server'ın desteklenen bizim yapılandırmasını kullanmak üzere yapılandırır. Kimlik sunucusu uygulama güvenlik kaygıları işlemek için çok güçlü ve Genişletilebilir bir çerçeve olan ancak hakkında bilgi edinmek için en yaygın senaryolar için ihtiyaç duymayacağımız karmaşıklık çok sunan aynı zamanda, bu nedenle Seçtiğimiz kuralları kümesi ve yapılandırma seçenekleri düşünüyoruz sizin için iyi bir başlangıç noktasıdır. Kimlik Doğrulamanızın ihtiyaçları değiştiğinde sonra şekilde gereksinimlerinize uyacak şekilde özelleştirebilirsiniz gücünden türde kimlik sunucusu için yine de kullanılabilir.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
Bu yardımcı yöntemi, uygulama için bir ilke düzeni varsayılan kimlik doğrulaması işleyici yapılandırır. İlke kimlik herhangi bir alt kimlik URL'si alanında Git tüm istekleri işlemeye izin vermek için yapılandırılmış "/ kimlik" ve diğer tüm istekleri işleyen JwtBearerHandler izin vermek için.
Bu yöntem kaydeder Addionally bir `<<ApplicationName>>API` kimlik sunucusu varsayılan kapsamı ile API kaynak `<<ApplicationName>>API` ve uygulama için kimlik sunucusu tarafından verilen belirteçleri doğrulamak için JWT taşıyıcı belirteç ara yazılımı yapılandırır.

### <a name="sampledatacontroller"></a>SampleDataController
Dosyayı Controllers\SampleDataController.cs baktığımızda, biz inceleyebileceğiniz `[Authorize]` kullanıcı yetki verilmesi gerektiğini belirten sınıfına uygulanan bir öznitelik tabanlı kaynağa erişmek için varsayılan ilke. Varsayılan yetkilendirme ilkesi tarafından ayarlanan varsayılan kimlik doğrulama düzeni kullanmak üzere yapılandırılmış olur `AddIdentityServerJwt` biz yukarıda bahsedilen ilke düzenine JwtBearer işleyici yapma gibi yardımcı yöntemi tarafından varsayılan işleyici için yapılandırılmış Uygulama istekleri.

### <a name="applicationdbcontext"></a>ApplicationDbContext
Data\ApplicationDbContext.cs dosyasında baktığımızda kullandığımız bu durumun Kimlik'te ApiAuthorizationDbContext (Identitydbcontext daha türetilmiş bir sınıftan) genişletir aynı Dbcontext'e görebiliriz kimlik sunucusu için şemayı içerecek şekilde.
Veritabanı şeması tam denetim istiyoruz biz oluşabilir yalnızca kullanılabilir kimlik DbContext sınıflarının birinden devralan ve bağlam çağırarak kimlik şemayı içerecek şekilde yapılandırmak `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` üzerinde `OnModelCreating` yöntemi.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
Biz bakarsanız dosyasını şu uç nokta görebilirsiniz Controllers\OidcConfigurationController.cs olduğumuz istemci kullanması gereken OIDC parametreleri hizmet ayakta.

### <a name="appsettingsjson"></a>appsettings.json
Proje kökündeki appsettings.json dosyasını baktığımızda, yeni bir görebiliriz `IdentityServer` listesini açıklayan bölümü yapılandırılmış istemciler ve tek bir istemci olduğunu görebiliriz. İstemci uygulama adına karşılık gelir ve oAuth ClientID parametresi için kural olarak eşleştirilir. Yapılandırmakta olduğunuz uygulamanın türüne profili gösterir ve bunu dahili olarak sunucu için yapılandırma işlemini basitleştirmek sürücü kuralları için kullanırız. Birkaç profil kullanılabilir aşağıdaki bölümde açıklanan vardır.

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
Biz appsettings bakarsanız. Development.JSON dosya projenin kökünde, yeni bir görebiliriz `IdentityServer` Belirteçleri imzalamak için kullanıyoruz anahtar açıklayan bölümü. Üretime dağıtırken bir anahtar sağlanır ve aşağıda açıklandığı gibi uygulamanın yanı sıra dağıtılması gerekir.

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Angular uygulamasını genel açıklaması
Kimlik doğrulama ve yetkilendirme Angular şablonu API desteği, kendi Angular modülünde yaşar. ClientApp\src\api yetkilendirme ve bunun altında aşağıdaki öğelerden oluşur:
* 3 bileşenler:
  * Oturum açma bileşeni: Uygulama için oturum açma akışını yönetir.
  * Oturum kapatma bileşeni: Uygulama oturum kapatma akış işler.
  * Oturum açma menü bileşeni: Geçerli görüntüleyen bir pencere öğesi kullanıcı profili yönetip oturumunuzu bağlantıları ile kullanıcının kimlik doğrulamasını ya da oturum açın veya kullanıcı kimliği doğrulanmamış olduğunda kaydetmek için bağlar.
* Bir rota guard `AuthorizeGuard` yolları eklenebilir ve rota gitmeden önce doğrulanmasını gerektirir.
* Bir http dinleyiciyi `AuthorizeInterceptor` kullanıcının kimliği doğrulandığında, API'yi hedefleyen giden HTTP isteklerini erişim belirtecini ekler.
* Bir hizmet `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için uygulama tüketim için kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.
* Yollar tanımlayan bir angular modülü, uygulamanın kimlik doğrulaması bölümlerine ile ilişkili ve oturum açma menü bileşeni, dinleyiciyi, koruma ve uygulama geri kalanından hizmetini kullanıma sunar.

## <a name="general-description-of-the-react-application"></a>React uygulamanın genel açıklaması
Kimlik doğrulama ve yetkilendirme ClientApp\src\components\api authorization\ ve altında React şablonu olduğu API desteği aşağıdaki öğelerden oluşur:
* 4 bileşenler:
  * Oturum açma bileşeni: Uygulama için oturum açma akışını yönetir.
  * Oturum kapatma bileşeni: Uygulama oturum kapatma akış işler.
  * Oturum açma menü bileşeni: Geçerli görüntüleyen bir pencere öğesi kullanıcı profili yönetip oturumunuzu bağlantıları ile kullanıcının kimlik doğrulamasını ya da oturum açın veya kullanıcı kimliği doğrulanmamış olduğunda kaydetmek için bağlar.
  * AuthorizeRoute: Bileşen işlemeden önce doğrulanması bir kullanıcı gerektiren bir yol bileşeni bileşen parametresinde belirtilen.
  * Dışarı aktarılan bir `authService` sınıfının örneğini `AuthorizeService` kimlik doğrulama işlemi alt düzey ayrıntıları işler ve geri kalanı için uygulama tüketim için kimliği doğrulanmış kullanıcı hakkında bilgileri gösterir.

Çözüm ana bileşenleri gördük, size belirli bir uygulama için tek tek senaryoları göz atabilirsiniz:

## <a name="requiring-authorization-on-a-new-api"></a>Yeni bir API üzerinde gerektiren yetkilendirme
Sistem, yetkilendirme için yeni API'leri zorunlu tutmak için basit hale getirmek için hazır yapılandırıldı. Bunu yapmak için yalnızca yeni bir denetleyici oluşturma ve ekleme `[Authorize]` denetleyicideki tüm eylem veya denetleyici sınıfı özniteliği.

## <a name="protecting-a-client-side-route-angular"></a>Bir istemci-tarafı yol (Angular) koruma
İstemci tarafı rota koruma authorize guard bir rota yapılandırırken çalıştırılacak cf listesine ekleyerek gerçekleştirilir. Örneğin, verileri getirme rota ana uygulama angular modülü içinde nasıl yapılandırıldığını görebilirsiniz:

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Bir rota koruma gerçek bir uç nokta koruma sağlamaz bahsetmek önemlidir (yine de gerektiren bir `[Authorize]` özniteliğinin) ancak bu, kimliği doğrulanmamış olduğunda belirli istemci tarafı yol gezinme yalnızca kullanıcının engeller.

## <a name="authenticate-api-requests-angular"></a>API isteklerinin (Angular) kimlik doğrulaması

Kimlik doğrulaması tarafında barındırılan API'ler isteklerini uygulama otomatik olarak uygulama tarafından tanımlanmış HTTP istemci dinleyiciyi kullanılarak gerçekleştirilir.

## <a name="protect-a-client-side-route-react"></a>Bir istemci-tarafı yol (React) koruma

İstemci tarafı rota korumak yerine düz rota bileşen AuthorizeRoute bileşeni kullanılarak yapılır. Örneğin, verileri getirme rota uygulama bileşeni içinde nasıl yapılandırıldığını görebilirsiniz:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Bir rota koruma gerçek bir uç nokta koruma sağlamaz bahsetmek önemlidir (yine de gerektiren bir `[Authorize]` özniteliğinin) ancak bu, kimliği doğrulanmamış olduğunda belirli istemci tarafı yol gezinme yalnızca kullanıcının engeller.

## <a name="authenticate-api-requests-react"></a>API isteklerinin (React) kimlik doğrulaması

React ile isteklerine kimlik doğrulaması yapılır ilk içeri aktararak `authService` gelen örnek `AuthorizeService` authService erişim belirteci alma ve ardından aşağıda gösterildiği gibi isteği ekleniyor. React bileşenlerde bu genellikle componentDidMount yaşam döngüsü yöntemi veya sonucunda bazı kullanıcı etkileşimi sonucunda gerçekleştirilir.

### <a name="import-the-authservice-into-your-component"></a>Bileşeniniz authService içe

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Alın ve yanıtı erişim belirteci ekleme

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a>Üretime dağıtma

Çeşitli kaynakları sağlamak için ihtiyacımız olan üretim uygulamasına dağıtmak için:
* Kimlik kullanıcı hesaplarını ve kimlik sunucusu depolamak için bir veritabanı verir.
* Belirteçleri imzalamak için kullanmak üzere bir üretim sertifika.
  * Bu sertifika için belirli gereksinimler yoktur; otomatik olarak imzalanan bir sertifika veya bir CA yetkilisi sağlanan bir sertifika olabilir.
  * Powershell veya openssl gibi standart araçlar aracılığıyla oluşturulabilir.
  * Hedef makinelerde sertifika deposuna yüklü veya güçlü bir parola ile bir pfx dosyası olarak dağıtılabilir.

### <a name="example-deploy-into-azure-websites"></a>Örnek: Azure Web dağıtma

Bu bölümde sertifika depolama alanında depolanan bir sertifika kullanarak Azure Web siteleri için uygulamayı dağıtmak için kullanacağız. Biz sertifika deposundan bir certicate yüklemek için uygulamayı değiştirmeniz gerekir. Bunu yapmak için app service planı biz daha sonraki bir adımda yapılandırdığınızda standart katmanda en az olması gerekir. Uygulamamızı içinde biz yalnızca anahtar ayrıntılarını içerecek şekilde appsettings.json IdentityServer bölümüne değiştirmeniz gerekir:
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* Sertifika adı özelliği ayırt edici konu sertifikasının karşılık gelir.
* Depolama konumu (CurrentUrser veya LocalMachine) sertifika yükleneceği temsil eder.
* Depo adını, sertifika, bu durumda kullanıcı Kişisel depoya işaret depolandığı sertifika deposunun adını temsil eder.

Yer alan adımları uygulayarak bir uygulamayı Azure Web siteleri'ne dağıtma [uygulamasını Azure'a dağıtma](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) gerekli Azure kaynakları oluşturmak ve uygulamayı üretime dağıtın.

Bunu yaptıktan sonra uygulamayı Azure'a dağıtılır, ancak yine de uygulama tarafından kullanılacak bir sertifika ayarlamak üzere ihtiyacımız henüz tamamen işlevsel değildir. Bunu yapmak için açıklanan adımları izleyin ve kullanmak için olduğumuz sertifikanın parmak izi sağlamak ihtiyacımız [sertifikalarınızı yüklemek](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Bu adımları SSL bahsetmek yoktur "Özel Sertifikalar" bölümünde portalında nerede biz uygulamamız ile kullanmak için sağlanan sertifika karşıya yükleyebilirsiniz.

Bu adımdan sonra biz uygulamamız yeniden başlatabiliyor olmanız ve tamamen işlevsel olması gerekir.

## <a name="other-configuration-options"></a>Diğer yapılandırma seçenekleri
Desteğimiz için API yetkilendirme kuralları, varsayılan değerleri ve tek sayfa uygulamaları deneyimini kolaylaştırmak için geliştirmeler kümesiyle kimlik sunucusu üzerinde oluşturur. Needless çok deyin sunuyoruz tümleştirmeler senaryonuz erişmezse kimlik Server'ın arka planda kullanılabilir. Destek kuruluşumuz tarafından dağıtılan tüm uygulamalar burada oluşturulan ve "Birinci taraf" uygulamaları dediğimiz üzerinde odaklanır. Bu nedenle onay ve Federasyon gibi şeyler için destek sunuyoruz yok. Bu senaryolar için Bizim önerimiz kimlik sunucusunu kendi belgelerini izleyin kullanmaktır.

### <a name="application-profiles"></a>Uygulama profilleri
Uygulama profilleri, kendi parametrelerini daha tanımlamanız uygulamalar için önceden tanımlanmış yapılandırmalardır. Şu anda iki profili destekliyoruz:
* IdentityServerSPA: Kimlik sunucusu tek bir birim olarak birlikte barındırılan bir tek sayfalı uygulama temsil eder.
  * Varsayılan olarak redirect_uri `/authentication/login-callback`.
  * Varsayılan olarak post_logout_redirect_uri `/authentication/logout-callback`.
  * Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.
  * İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).
  * İzin verilen yanıt modu `fragment`.
* SPA: Kimlik sunucusu ile barındırılmayan bir tek sayfalı uygulama temsil eder.
  * Kapsamları kümesini içeren `openid`, `profile`ve uygulama API'leri için tanımlanan her kapsam.
  * İzin verilen OIDC yanıt türleri kümesi `id_token token` veya bunların her birini tek tek (`id_token`, `token`).
  * İzin verilen yanıt modu `fragment`.
* IdentityServerJwt: Yanı sıra barındırılan bir API ile kimlik sunucusunu temsil eder.
  * Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.
* API: Değil barındırılan bir API ile kimlik sunucusunu temsil eder.
  * Uygulama, uygulama adı için varsayılan olarak tek bir kapsam sahip olacak şekilde yapılandırılır.

### <a name="configuration-through-appsettings"></a>AppSettings aracılığıyla yapılandırma
Uygulamaları yapılandırma sistemimiz istemciler veya kaynaklar listesine sırasıyla ekleyerek için bunları yapılandırabiliriz. 

İstemciler yapılandırırken yapılandırabiliriz `redirect_uri` ve `post_logout_redirect_uri` aşağıda gösterildiği gibi:
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

Kaynakları yapılandırırken size kapsamları kaynak için aşağıda gösterildiği gibi yapılandırabilirsiniz:
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a>Kod ile yapılandırma
Biz, istemciler ve kaynakları bir aşırı yüklemesini seçeneklerini yapılandırmak için bir eylemde AddApiAuthorization kullanarak kod aracılığıyla da yapılandırabilirsiniz.
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
