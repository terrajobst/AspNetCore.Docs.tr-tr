---
title: ASP.NET Core sertifika kimlik doğrulamasını yapılandırma
author: blowdart
description: Sertifika kimlik doğrulaması için IIS ve HTTP.sys içinde ASP.NET Core yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: barry.dorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 37567128531187bfe7dd6c9f5aa990226e70f35f
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837555"
---
# <a name="overview"></a>Genel Bakış

`Microsoft.AspNetCore.Authentication.Certificate` benzer şekilde bir uygulaması içeren [sertifika kimlik doğrulaması](https://tools.ietf.org/html/rfc5246#section-7.4.4) ASP.NET Core için. Şimdiye kadar bir ASP.NET Core alır önce sertifika kimlik doğrulaması TLS düzeyinde, uzun'olmuyor. Bu sertifikayı doğrular ve ardından, nerede, giderebilir Bu sertifika için bir olay verir. bir kimlik doğrulama işleyicisi daha doğru bir şekilde, bir `ClaimsPrincipal`. 

[Ana yapılandırma](#configure-your-host-to-require-certificates) sertifika kimlik doğrulaması için IIS, Kestrel, Azure Web Apps veya başka bir runbook'tan kullanmakta olduğunuz olması.

## <a name="get-started"></a>Kullanmaya başlayın

Bir HTTPS sertifikası alın, bunu, uygulama ve [ana yapılandırma](#configure-your-host-to-require-certificates) sertifikaları gerektirmek için.

Web uygulamanızda bir başvuru ekleyin `Microsoft.AspNetCore.Authentication.Certificate` paket. Ardından `Startup.Configure` yöntemi, çağrı `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` ile seçeneklerinizi için temsilci sağlayarak `OnCertificateValidated` gönderdiği istekleri ile istemci sertifikası herhangi ek doğrulama yapmak için. Bilgileri Aç bir `ClaimsPrincipal` ve açık ayarının `context.Principal` özelliği.

Kimlik doğrulama başarısız olursa, bu işleyicisini döndürür bir `403 (Forbidden)` yanıt yerine bir `401 (Unauthorized)`, bekleyebileceğiniz gibi. Mantık, kimlik doğrulaması sırasında ilk TLS bağlantı olacağını ' dir. İşleyici ulaştığında zamanına göre çok geç. Bağlantı bir sertifikayla anonim bir bağlantıdan yükseltmek için hiçbir yolu yoktur.

Ayrıca `app.UseAuthentication();` içinde `Startup.Configure` yöntemi. Aksi takdirde, HttpContext.User ayarlanmaz `ClaimsPrincipal` sertifikadan oluşturulur. Örneğin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

Yukarıdaki örnekte, sertifika kimlik doğrulaması eklemek için varsayılan yolu gösterir. Ortak sertifika özellikleri kullanarak bir kullanıcı asıl işleyicisi oluşturur.

## <a name="configure-certificate-validation"></a>Sertifika doğrulaması'nı yapılandırma

`CertificateAuthenticationOptions` İşleyicisi sunucusundaki bir sertifikanın gerçekleştirmesi gereken en düşük doğrulamaları olan bazı yerleşik doğrulamaları vardır. Bu ayarların her biri varsayılan olarak etkindir.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = zincirleme, SelfSigned ya da tüm (zincirleme | SelfSigned)

Bu denetim yalnızca uygun sertifika türü izin verildiğini doğrular.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Bu denetim, istemci tarafından sunulan sertifika istemci kimlik doğrulaması genişletilmiş anahtar kullanımı (EKU) veya EKU listelenmez hiç sahip olduğunu doğrular. EKU yok belirtilirse belirtimleri söyleyin gibi tüm EKU'larına geçerli sayılan.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Bu onay, sertifikanın geçerlilik süresi olduğunu doğrular. Her bir istek üzerinde geçerli bir oturum sırasında bu iletildiğinde geçerli bir sertifika süresinin dolmadığından emin işleyici sağlar.

### <a name="revocationflag"></a>RevocationFlag

Hangi sertifikaların zincirinde belirten bir bayrak iptali için denetlenir.

İptal denetimlerini yalnızca sertifikasının bir kök sertifikaya zincir gerçekleştirilir.

### <a name="revocationmode"></a>revocationMode

İptal denetimlerini nasıl gerçekleştirileceğini belirten bir bayrak.

Sertifika yetkilisi bağlantı kurulurken bir çevrimiçi denetimi belirtme uzun gecikmelere neden olabilir.

İptal denetimlerini yalnızca sertifikasının bir kök sertifikaya zincir gerçekleştirilir.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Yalnızca belirli yollardaki bir sertifika istemek için uygulamamı yapılandırabilirim?

Bu mümkün değildir. Bu nedenle hiçbir istek alanlara göre kapsam mümkün değildir, ilk isteği bu bağlantıda alınmadan önce HTTPS iletişiminin başlangıç, sunucu tarafından işlemin tamamlandığını sertifika exchange yapılır unutmayın.

## <a name="handler-events"></a>İşleyici olayları

İki olay işleyicisi sahiptir:

* `OnAuthenticationFailed` &ndash; Bir özel durum kimlik doğrulaması sırasında gerçekleşir ve react sağlar çağrılır.
* `OnCertificateValidated` &ndash; Doğrulama başarılı oldu sertifika doğrulandıysa ve varsayılan sorumlu oluşturulduktan sonra çağrılır. Bu olay, kendi doğrulama işlemini yapabilir ve artırabilir veya asıl değiştirin olanak tanır. İçin örnekleri şunlardır:
  * Sertifika hizmetlerinize biliniyorsa belirleme.
  * Kendi asıl oluşturma. Aşağıdaki örnekte göz önünde bulundurun `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

Gelen sertifika, ek doğrulama karşılamadığında bulursanız, çağrı `context.Fail("failure reason")` bir başarısızlık nedeniyle.

Gerçek işlevleri, büyük olasılıkla bir veritabanı veya başka bir kullanıcı deposu türü bağlayan bir bağımlılık ekleme kayıtlı bir hizmeti çağırmak isteyebilirsiniz. Hizmetiniz, temsilciye iletilen bağlamını kullanarak erişin. Aşağıdaki örnekte göz önünde bulundurun `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

Kavramsal olarak, doğrulama sertifikasının bir yetkilendirme konusudur. Örneğin bir denetimi, ekleme, bir veren veya parmak izi bir yetkilendirme ilkesi, yerine iç `OnCertificateValidated`, mükemmel bir şekilde kabul edilebilir.

## <a name="configure-your-host-to-require-certificates"></a>Sertifikaları gerektirmek için ana yapılandırma

### <a name="kestrel"></a>Kestrel

İçinde *Program.cs*, Kestrel aşağıdaki gibi yapılandırın:

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a>IIS

IIS Yöneticisi'nde aşağıdaki adımları tamamlayın:

1. Sitenizi seçin **bağlantıları** sekmesi.
1. Çift **SSL ayarları** seçeneğini **özellikler görünümü** penceresi.
1. Denetleyin **SSL iste** onay kutusunu seçip **gerektiren** radyo düğmesinin **istemci sertifikaları** bölümü.

![IIS istemci sertifika ayarları](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure ve özel web Ara sunucuları

Bkz: [barındırma ve dağıtma belgeleri](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) ara yazılım iletme sertifikası yapılandırma için.
