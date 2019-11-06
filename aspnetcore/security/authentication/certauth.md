---
title: ASP.NET Core sertifika kimlik doğrulamasını yapılandırma
author: blowdart
description: IIS ve HTTP. sys için ASP.NET Core sertifika kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 11/05/2019
uid: security/authentication/certauth
ms.openlocfilehash: 081935e6e6248b5fe9b7bf4cd966dc73761d2ec1
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634047"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="d7cc7-103">ASP.NET Core sertifika kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d7cc7-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="d7cc7-104">`Microsoft.AspNetCore.Authentication.Certificate`, ASP.NET Core için [sertifika kimlik doğrulamasına](https://tools.ietf.org/html/rfc5246#section-7.4.4) benzer bir uygulama içerir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="d7cc7-105">Sertifika kimlik doğrulaması TLS düzeyinde gerçekleşir ve bu süre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="d7cc7-106">Daha doğru, bu, sertifikayı doğrulayan bir kimlik doğrulama işleyicisidir ve bu sertifikayı bir `ClaimsPrincipal`çözümleyebileceğiniz bir olay verir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="d7cc7-107">[Ana bilgisayarınızı](#configure-your-host-to-require-certificates) sertifika kimlik doğrulaması için yapılandırın, BT IIS, Kestrel, Azure Web Apps veya kullandığınız başka herhangi bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="d7cc7-108">Proxy ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="d7cc7-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="d7cc7-109">Sertifika kimlik doğrulaması, genellikle bir ara sunucu veya yük dengeleyicinin istemciler ve sunucular arasındaki trafiği işleyememesi durumunda kullanılan, durum bilgisi olan bir senaryodur</span><span class="sxs-lookup"><span data-stu-id="d7cc7-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="d7cc7-110">Bir ara sunucu veya yük dengeleyici kullanılıyorsa, sertifika kimlik doğrulaması yalnızca proxy veya yük dengeleyici için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="d7cc7-111">Kimlik doğrulamasını işler.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-111">Handles the authentication.</span></span>
* <span data-ttu-id="d7cc7-112">Kullanıcı kimlik doğrulama bilgilerini uygulamaya geçirir (örneğin, bir istek üstbilgisinde), kimlik doğrulama bilgileri üzerinde davranır.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="d7cc7-113">Proxy 'lerin ve yük dengeleyicilerin kullanıldığı ortamlarda sertifika kimlik doğrulamasına alternatif olarak, OpenID Connect (OıDC) ile Federasyon Hizmetleri (ADFS) Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="d7cc7-114">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="d7cc7-114">Get started</span></span>

<span data-ttu-id="d7cc7-115">Bir HTTPS sertifikası alın, uygulayın ve [ana bilgisayarınızı](#configure-your-host-to-require-certificates) sertifika gerektirecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="d7cc7-116">Web uygulamanızda `Microsoft.AspNetCore.Authentication.Certificate` paketine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="d7cc7-117">Daha sonra `Startup.ConfigureServices` yönteminde, `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);`, isteklerle gönderilen istemci sertifikası üzerinde herhangi bir ek doğrulama yapmak üzere `OnCertificateValidated` için bir temsilci sağlayarak seçeneklerinizi çağırın.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="d7cc7-118">Bu bilgileri bir `ClaimsPrincipal` açın ve `context.Principal` özelliğinde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="d7cc7-119">Kimlik doğrulaması başarısız olursa, bu işleyici, bekleneceğiniz gibi bir `401 (Unauthorized)`yerine `403 (Forbidden)` yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="d7cc7-120">Bu durum, kimlik doğrulamanın ilk TLS bağlantısı sırasında gerçekleşme nedendir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="d7cc7-121">İşleyiciye ulaştığında, çok geç olur.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="d7cc7-122">Anonim bir bağlantıyla bir sertifikayla bir bağlantıyı yükseltmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="d7cc7-123">Ayrıca, `Startup.Configure` yöntemine `app.UseAuthentication();` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="d7cc7-124">Aksi halde, HttpContext. User, sertifikadan oluşturulan `ClaimsPrincipal` olarak ayarlanmayacak.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="d7cc7-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-125">For example:</span></span>

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

<span data-ttu-id="d7cc7-126">Önceki örnekte sertifika kimlik doğrulaması eklemenin varsayılan yolu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="d7cc7-127">İşleyici, ortak sertifika özelliklerini kullanarak bir Kullanıcı sorumlusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="d7cc7-128">Sertifika doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d7cc7-128">Configure certificate validation</span></span>

<span data-ttu-id="d7cc7-129">`CertificateAuthenticationOptions` işleyicisinde, bir sertifikada gerçekleştirmeniz gereken en düşük doğrulamalar olan yerleşik doğrulamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="d7cc7-130">Bu ayarların her biri varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="d7cc7-131">AllowedCertificateTypes = zincirleme, SelfSigned veya All (zincirleme | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="d7cc7-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="d7cc7-132">Bu denetim yalnızca uygun sertifika türüne izin verildiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="d7cc7-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="d7cc7-133">ValidateCertificateUse</span></span>

<span data-ttu-id="d7cc7-134">Bu denetim, istemci tarafından sunulan sertifikanın Istemci kimlik doğrulaması genişletilmiş anahtar kullanımı (EKU) olduğunu veya hiç EKU olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="d7cc7-135">Belirtimlerde bir EKU belirtilmemişse, tüm EKU 'lar geçerli kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="d7cc7-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="d7cc7-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="d7cc7-137">Bu denetim, sertifikanın geçerlilik süresi içinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="d7cc7-138">Her istekte işleyici, geçerli oturumu sırasında sunulmadığı zaman geçerli olmayan bir sertifikanın süresinin dolmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="d7cc7-139">Revocationbayrağı</span><span class="sxs-lookup"><span data-stu-id="d7cc7-139">RevocationFlag</span></span>

<span data-ttu-id="d7cc7-140">Zincirdeki hangi sertifikaların iptal için denetleneceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="d7cc7-141">İptal denetimleri yalnızca sertifika bir kök sertifikaya zincirleme yapıldığında gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="d7cc7-142">Revocationmodu</span><span class="sxs-lookup"><span data-stu-id="d7cc7-142">RevocationMode</span></span>

<span data-ttu-id="d7cc7-143">İptal denetimlerinin nasıl gerçekleştirileceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="d7cc7-144">Bir çevrimiçi denetim belirtildiğinde, sertifika yetkilisi ile bağlantı kurulduğunda uzun bir gecikmeyle sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="d7cc7-145">İptal denetimleri yalnızca sertifika bir kök sertifikaya zincirleme yapıldığında gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="d7cc7-146">Uygulamamı yalnızca belirli yollarda sertifika gerektirecek şekilde yapılandırabilir miyim?</span><span class="sxs-lookup"><span data-stu-id="d7cc7-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="d7cc7-147">Bu mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-147">This isn't possible.</span></span> <span data-ttu-id="d7cc7-148">Sertifika değişimi 'nin HTTPS konuşması başlangıcı olduğunu unutmayın, bu bağlantı üzerinde ilk istek alınmadan önce sunucu tarafından gerçekleştirilir, böylece herhangi bir istek alanı temelinde kapsam yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="d7cc7-149">İşleyici olayları</span><span class="sxs-lookup"><span data-stu-id="d7cc7-149">Handler events</span></span>

<span data-ttu-id="d7cc7-150">İşleyicinin iki olayı vardır:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-150">The handler has two events:</span></span>

* <span data-ttu-id="d7cc7-151">`OnAuthenticationFailed` &ndash;, kimlik doğrulaması sırasında bir özel durum oluşursa çağrılır ve yanıt vermenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="d7cc7-152">Sertifika doğrulandıktan sonra `OnCertificateValidated` &ndash; çağrıldı, doğrulama geçildi ve bir varsayılan asıl oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="d7cc7-153">Bu olay kendi doğrulamayı gerçekleştirmenize ve sorumluyu artırabilir veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="d7cc7-154">Örnekler için şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-154">For examples include:</span></span>
  * <span data-ttu-id="d7cc7-155">Sertifikanın hizmetlerinize göre bilinip tanınmadığını belirleme.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="d7cc7-156">Kendi sorumlunuzu oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-156">Constructing your own principal.</span></span> <span data-ttu-id="d7cc7-157">`Startup.ConfigureServices`içinde aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="d7cc7-158">Gelen sertifikayı, ek doğrulamadan uymadığını fark ederseniz bir hata nedeniyle `context.Fail("failure reason")` çağırın.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="d7cc7-159">Gerçek işlevsellik için muhtemelen bir veritabanına veya diğer Kullanıcı Mağazası türüne bağlanan bağımlılık ekleme bölümünde kayıtlı bir hizmeti çağırmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="d7cc7-160">Temsilciniz 'e geçirilen bağlamı kullanarak hizmetinize erişin.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="d7cc7-161">`Startup.ConfigureServices`içinde aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="d7cc7-162">Kavramsal olarak, sertifikanın doğrulanması bir yetkilendirme konusudur.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="d7cc7-163">`OnCertificateValidated`içinde değil, yetkilendirme ilkesindeki bir veren veya parmak izi gibi bir denetim eklemek, kusursuz kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="d7cc7-164">Ana bilgisayarınızı sertifika gerektirecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d7cc7-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="d7cc7-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d7cc7-165">Kestrel</span></span>

<span data-ttu-id="d7cc7-166">*Program.cs*' de, Kestrel ' yi aşağıdaki şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-166">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp

public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}

public static IHostBuilder CreateHostBuilder(string[] args)
{
    return Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                    webBuilder.ConfigureKestrel(o =>
                    {
                        o.ConfigureHttpsDefaults(o => o.ClientCertificateMode = ClientCertificateMode.RequireCertificate);
                    });
                });
}
```

### <a name="iis"></a><span data-ttu-id="d7cc7-167">IIS</span><span class="sxs-lookup"><span data-stu-id="d7cc7-167">IIS</span></span>

<span data-ttu-id="d7cc7-168">IIS Yöneticisi 'Nde aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="d7cc7-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="d7cc7-169">**Bağlantılar** sekmesinden sitenizi seçin.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="d7cc7-170">**Özellikler Görünümü** penceresinde **SSL ayarları** seçeneğine çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="d7cc7-171">**SSL gerektir** onay kutusunu Işaretleyin ve **istemci sertifikaları** bölümünde radyo **iste** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d7cc7-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![IIS 'de istemci sertifikası ayarları](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="d7cc7-173">Azure ve özel Web proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="d7cc7-173">Azure and custom web proxies</span></span>

<span data-ttu-id="d7cc7-174">Sertifika iletme ara yazılımını yapılandırma hakkında bilgi için bkz. [konak ve dağıtım belgeleri](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) .</span><span class="sxs-lookup"><span data-stu-id="d7cc7-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
