---
title: ASP.NET Core sertifika kimlik doğrulamasını yapılandırma
author: blowdart
description: IIS ve HTTP. sys için ASP.NET Core sertifika kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: bb375cf380175daf2399f3b56f543819ee5692b8
ms.sourcegitcommit: 07cd66e367d080acb201c7296809541599c947d1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039251"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a>ASP.NET Core sertifika kimlik doğrulamasını yapılandırma

`Microsoft.AspNetCore.Authentication.Certificate`ASP.NET Core için [sertifika kimlik doğrulamasına](https://tools.ietf.org/html/rfc5246#section-7.4.4) benzer bir uygulama içerir. Sertifika kimlik doğrulaması TLS düzeyinde gerçekleşir ve bu süre ASP.NET Core. Daha doğru, bu, sertifikayı doğrulayan bir kimlik doğrulama işleyicisidir ve bu sertifikayı bir ' a `ClaimsPrincipal`çözebileceğiniz bir olay verir. 

[Ana bilgisayarınızı](#configure-your-host-to-require-certificates) sertifika kimlik doğrulaması için yapılandırın, BT IIS, Kestrel, Azure Web Apps veya kullandığınız başka herhangi bir şeydir.

## <a name="proxy-and-load-balancer-scenarios"></a>Proxy ve yük dengeleyici senaryoları

Sertifika kimlik doğrulaması, genellikle bir ara sunucu veya yük dengeleyicinin istemciler ve sunucular arasındaki trafiği işleyememesi durumunda kullanılan, durum bilgisi olan bir senaryodur Bir ara sunucu veya yük dengeleyici kullanılıyorsa, sertifika kimlik doğrulaması yalnızca proxy veya yük dengeleyici için geçerlidir:

* Kimlik doğrulamasını işler.
* Kullanıcı kimlik doğrulama bilgilerini uygulamaya geçirir (örneğin, bir istek üstbilgisinde), kimlik doğrulama bilgileri üzerinde davranır.

Proxy 'lerin ve yük dengeleyicilerin kullanıldığı ortamlarda sertifika kimlik doğrulamasına alternatif olarak, OpenID Connect (OıDC) ile Federasyon Hizmetleri (ADFS) Active Directory.

## <a name="get-started"></a>Kullanmaya başlayın

Bir HTTPS sertifikası alın, uygulayın ve [ana bilgisayarınızı](#configure-your-host-to-require-certificates) sertifika gerektirecek şekilde yapılandırın.

Web uygulamanızda `Microsoft.AspNetCore.Authentication.Certificate` pakete bir başvuru ekleyin. Daha sonra `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` `OnCertificateValidated` yönteminde, isteklerle birlikte gönderilen istemci sertifikasında herhangi bir destek doğrulaması yapmak için bir temsilci sağlayan seçeneklerinizde çağrı yapın. `Startup.Configure` Bu bilgileri bir `ClaimsPrincipal` öğesine dönüştürün ve `context.Principal` özelliği üzerinde ayarlayın.

Kimlik doğrulaması başarısız olursa, bu işleyici, `403 (Forbidden)` bekleolabileceğiniz gibi `401 (Unauthorized)`bir yanıt döndürür. Bu durum, kimlik doğrulamanın ilk TLS bağlantısı sırasında gerçekleşme nedendir. İşleyiciye ulaştığında, çok geç olur. Anonim bir bağlantıyla bir sertifikayla bir bağlantıyı yükseltmenin bir yolu yoktur.

Yöntemine de `app.UseAuthentication();`ekleyin. `Startup.Configure` Aksi halde, HttpContext. User, sertifikadan oluşturulacak şekilde `ClaimsPrincipal` ayarlanmayacak. Örneğin:

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

Önceki örnekte sertifika kimlik doğrulaması eklemenin varsayılan yolu gösterilmektedir. İşleyici, ortak sertifika özelliklerini kullanarak bir Kullanıcı sorumlusu oluşturur.

## <a name="configure-certificate-validation"></a>Sertifika doğrulamasını yapılandırma

İşleyicide, `CertificateAuthenticationOptions` bir sertifikada gerçekleştirmeniz gereken en düşük doğrulamalar olan yerleşik doğrulamalar vardır. Bu ayarların her biri varsayılan olarak etkindir.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = zincirleme, SelfSigned veya All (zincirleme | SelfSigned)

Bu denetim yalnızca uygun sertifika türüne izin verildiğini doğrular.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Bu denetim, istemci tarafından sunulan sertifikanın Istemci kimlik doğrulaması genişletilmiş anahtar kullanımı (EKU) olduğunu veya hiç EKU olmadığını doğrular. Belirtimlerde bir EKU belirtilmemişse, tüm EKU 'lar geçerli kabul edilir.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Bu denetim, sertifikanın geçerlilik süresi içinde olduğunu doğrular. Her istekte işleyici, geçerli oturumu sırasında sunulmadığı zaman geçerli olmayan bir sertifikanın süresinin dolmamasını sağlar.

### <a name="revocationflag"></a>Revocationbayrağı

Zincirdeki hangi sertifikaların iptal için denetleneceğini belirten bayrak.

İptal denetimleri yalnızca sertifika bir kök sertifikaya zincirleme yapıldığında gerçekleştirilir.

### <a name="revocationmode"></a>Revocationmodu

İptal denetimlerinin nasıl gerçekleştirileceğini belirten bayrak.

Bir çevrimiçi denetim belirtildiğinde, sertifika yetkilisi ile bağlantı kurulduğunda uzun bir gecikmeyle sonuçlanabilir.

İptal denetimleri yalnızca sertifika bir kök sertifikaya zincirleme yapıldığında gerçekleştirilir.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Uygulamamı yalnızca belirli yollarda sertifika gerektirecek şekilde yapılandırabilir miyim?

Bu mümkün değildir. Sertifika değişimi 'nin HTTPS konuşması başlangıcı olduğunu unutmayın, bu bağlantı üzerinde ilk istek alınmadan önce sunucu tarafından gerçekleştirilir, böylece herhangi bir istek alanı temelinde kapsam yapılamaz.

## <a name="handler-events"></a>İşleyici olayları

İşleyicinin iki olayı vardır:

* `OnAuthenticationFailed`&ndash; Kimlik doğrulaması sırasında bir özel durum oluşursa ve işlem yapmanıza izin veriyorsa çağırılır.
* `OnCertificateValidated`&ndash; Sertifika doğrulandıktan sonra, doğrulama geçildi ve bir varsayılan asıl oluşturulur. Bu olay kendi doğrulamayı gerçekleştirmenize ve sorumluyu artırabilir veya değiştirmenize olanak sağlar. Örnekler için şunları içerir:
  * Sertifikanın hizmetlerinize göre bilinip tanınmadığını belirleme.
  * Kendi sorumlunuzu oluşturma. İçinde `Startup.ConfigureServices`aşağıdaki örneği göz önünde bulundurun:

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

Gelen sertifikayı, ek doğrulamadan uymadığını fark ederseniz bir hata nedeniyle çağırın `context.Fail("failure reason")` .

Gerçek işlevsellik için muhtemelen bir veritabanına veya diğer Kullanıcı Mağazası türüne bağlanan bağımlılık ekleme bölümünde kayıtlı bir hizmeti çağırmak isteyeceksiniz. Temsilciniz 'e geçirilen bağlamı kullanarak hizmetinize erişin. İçinde `Startup.ConfigureServices`aşağıdaki örneği göz önünde bulundurun:

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

Kavramsal olarak, sertifikanın doğrulanması bir yetkilendirme konusudur. Örneğin, bir yetkilendirme ilkesindeki `OnCertificateValidated`bir veren veya parmak izi gibi bir denetim eklemek, mükemmel bir kabul edilebilir.

## <a name="configure-your-host-to-require-certificates"></a>Ana bilgisayarınızı sertifika gerektirecek şekilde yapılandırma

### <a name="kestrel"></a>Kestrel

*Program.cs*' de, Kestrel ' yi aşağıdaki şekilde yapılandırın:

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

IIS Yöneticisi 'Nde aşağıdaki adımları uygulayın:

1. **Bağlantılar** sekmesinden sitenizi seçin.
1. **Özellikler Görünümü** penceresinde **SSL ayarları** seçeneğine çift tıklayın.
1. **SSL gerektir** onay kutusunu Işaretleyin ve **istemci sertifikaları** bölümünde radyo **iste** düğmesini seçin.

![IIS 'de istemci sertifikası ayarları](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure ve özel Web proxy 'leri

Sertifika iletme ara yazılımını yapılandırma hakkında bilgi için bkz. [konak ve dağıtım belgeleri](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) .
