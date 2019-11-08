---
title: ASP.NET Core sertifika kimlik doğrulamasını yapılandırma
author: blowdart
description: IIS ve HTTP. sys için ASP.NET Core sertifika kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 11/07/2019
uid: security/authentication/certauth
ms.openlocfilehash: 0db23c325f0b1f5a6500e3b2549db170e3df97c5
ms.sourcegitcommit: 68d804d60e104c81fe77a87a9af70b5df2726f60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73830713"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="d8395-103">ASP.NET Core sertifika kimlik doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d8395-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="d8395-104">`Microsoft.AspNetCore.Authentication.Certificate`, ASP.NET Core için [sertifika kimlik doğrulamasına](https://tools.ietf.org/html/rfc5246#section-7.4.4) benzer bir uygulama içerir.</span><span class="sxs-lookup"><span data-stu-id="d8395-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="d8395-105">Sertifika kimlik doğrulaması TLS düzeyinde gerçekleşir ve bu süre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8395-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="d8395-106">Daha doğru, bu, sertifikayı doğrulayan bir kimlik doğrulama işleyicisidir ve bu sertifikayı bir `ClaimsPrincipal`çözümleyebileceğiniz bir olay verir.</span><span class="sxs-lookup"><span data-stu-id="d8395-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="d8395-107">[Ana bilgisayarınızı](#configure-your-host-to-require-certificates) sertifika kimlik doğrulaması için yapılandırın, BT IIS, Kestrel, Azure Web Apps veya kullandığınız başka herhangi bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="d8395-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="d8395-108">Proxy ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="d8395-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="d8395-109">Sertifika kimlik doğrulaması, genellikle bir ara sunucu veya yük dengeleyicinin istemciler ve sunucular arasındaki trafiği işleyememesi durumunda kullanılan, durum bilgisi olan bir senaryodur</span><span class="sxs-lookup"><span data-stu-id="d8395-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="d8395-110">Bir ara sunucu veya yük dengeleyici kullanılıyorsa, sertifika kimlik doğrulaması yalnızca proxy veya yük dengeleyici için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="d8395-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="d8395-111">Kimlik doğrulamasını işler.</span><span class="sxs-lookup"><span data-stu-id="d8395-111">Handles the authentication.</span></span>
* <span data-ttu-id="d8395-112">Kullanıcı kimlik doğrulama bilgilerini uygulamaya geçirir (örneğin, bir istek üstbilgisinde), kimlik doğrulama bilgileri üzerinde davranır.</span><span class="sxs-lookup"><span data-stu-id="d8395-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="d8395-113">Proxy 'lerin ve yük dengeleyicilerin kullanıldığı ortamlarda sertifika kimlik doğrulamasına alternatif olarak, OpenID Connect (OıDC) ile Federasyon Hizmetleri (ADFS) Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d8395-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="d8395-114">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="d8395-114">Get started</span></span>

<span data-ttu-id="d8395-115">Bir HTTPS sertifikası alın, uygulayın ve [ana bilgisayarınızı](#configure-your-host-to-require-certificates) sertifika gerektirecek şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="d8395-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="d8395-116">Web uygulamanızda `Microsoft.AspNetCore.Authentication.Certificate` paketine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d8395-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="d8395-117">Daha sonra `Startup.ConfigureServices` yönteminde, `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);`, isteklerle gönderilen istemci sertifikası üzerinde herhangi bir ek doğrulama yapmak üzere `OnCertificateValidated` için bir temsilci sağlayarak seçeneklerinizi çağırın.</span><span class="sxs-lookup"><span data-stu-id="d8395-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="d8395-118">Bu bilgileri bir `ClaimsPrincipal` açın ve `context.Principal` özelliğinde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d8395-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="d8395-119">Kimlik doğrulaması başarısız olursa, bu işleyici, bekleneceğiniz gibi bir `401 (Unauthorized)`yerine `403 (Forbidden)` yanıtı döndürür.</span><span class="sxs-lookup"><span data-stu-id="d8395-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="d8395-120">Bu durum, kimlik doğrulamanın ilk TLS bağlantısı sırasında gerçekleşme nedendir.</span><span class="sxs-lookup"><span data-stu-id="d8395-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="d8395-121">İşleyiciye ulaştığında, çok geç olur.</span><span class="sxs-lookup"><span data-stu-id="d8395-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="d8395-122">Anonim bir bağlantıyla bir sertifikayla bir bağlantıyı yükseltmenin bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="d8395-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="d8395-123">Ayrıca, `Startup.Configure` yöntemine `app.UseAuthentication();` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d8395-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="d8395-124">Aksi takdirde `HttpContext.User`, sertifikadan oluşturulan `ClaimsPrincipal` olarak ayarlanmayacak.</span><span class="sxs-lookup"><span data-stu-id="d8395-124">Otherwise, the `HttpContext.User` will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="d8395-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d8395-125">For example:</span></span>

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

<span data-ttu-id="d8395-126">Önceki örnekte sertifika kimlik doğrulaması eklemenin varsayılan yolu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="d8395-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="d8395-127">İşleyici, ortak sertifika özelliklerini kullanarak bir Kullanıcı sorumlusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d8395-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="d8395-128">Sertifika doğrulamasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d8395-128">Configure certificate validation</span></span>

<span data-ttu-id="d8395-129">`CertificateAuthenticationOptions` işleyicisinde, bir sertifikada gerçekleştirmeniz gereken en düşük doğrulamalar olan yerleşik doğrulamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="d8395-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="d8395-130">Bu ayarların her biri varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="d8395-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="d8395-131">AllowedCertificateTypes = zincirleme, SelfSigned veya All (zincirleme | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="d8395-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="d8395-132">Bu denetim yalnızca uygun sertifika türüne izin verildiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="d8395-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="d8395-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="d8395-133">ValidateCertificateUse</span></span>

<span data-ttu-id="d8395-134">Bu denetim, istemci tarafından sunulan sertifikanın Istemci kimlik doğrulaması genişletilmiş anahtar kullanımı (EKU) olduğunu veya hiç EKU olmadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="d8395-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="d8395-135">Belirtimlerde bir EKU belirtilmemişse, tüm EKU 'lar geçerli kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="d8395-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="d8395-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="d8395-137">Bu denetim, sertifikanın geçerlilik süresi içinde olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="d8395-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="d8395-138">Her istekte işleyici, geçerli oturumu sırasında sunulmadığı zaman geçerli olmayan bir sertifikanın süresinin dolmamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="d8395-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="d8395-139">Revocationbayrağı</span><span class="sxs-lookup"><span data-stu-id="d8395-139">RevocationFlag</span></span>

<span data-ttu-id="d8395-140">Zincirdeki hangi sertifikaların iptal için denetleneceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="d8395-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="d8395-141">İptal denetimleri yalnızca sertifika bir kök sertifikaya zincirleme yapıldığında gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="d8395-142">Revocationmodu</span><span class="sxs-lookup"><span data-stu-id="d8395-142">RevocationMode</span></span>

<span data-ttu-id="d8395-143">İptal denetimlerinin nasıl gerçekleştirileceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="d8395-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="d8395-144">Bir çevrimiçi denetim belirtildiğinde, sertifika yetkilisi ile bağlantı kurulduğunda uzun bir gecikmeyle sonuçlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="d8395-145">İptal denetimleri yalnızca sertifika bir kök sertifikaya zincirleme yapıldığında gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="d8395-146">Uygulamamı yalnızca belirli yollarda sertifika gerektirecek şekilde yapılandırabilir miyim?</span><span class="sxs-lookup"><span data-stu-id="d8395-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="d8395-147">Bu mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="d8395-147">This isn't possible.</span></span> <span data-ttu-id="d8395-148">Sertifika değişimi 'nin HTTPS konuşması başlangıcı olduğunu unutmayın, bu bağlantı üzerinde ilk istek alınmadan önce sunucu tarafından gerçekleştirilir, böylece herhangi bir istek alanı temelinde kapsam yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="d8395-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="d8395-149">İşleyici olayları</span><span class="sxs-lookup"><span data-stu-id="d8395-149">Handler events</span></span>

<span data-ttu-id="d8395-150">İşleyicinin iki olayı vardır:</span><span class="sxs-lookup"><span data-stu-id="d8395-150">The handler has two events:</span></span>

* <span data-ttu-id="d8395-151">`OnAuthenticationFailed` &ndash;, kimlik doğrulaması sırasında bir özel durum oluşursa çağrılır ve yanıt vermenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="d8395-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="d8395-152">Sertifika doğrulandıktan sonra `OnCertificateValidated` &ndash; çağrıldı, doğrulama geçildi ve bir varsayılan asıl oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d8395-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="d8395-153">Bu olay kendi doğrulamayı gerçekleştirmenize ve sorumluyu artırabilir veya değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d8395-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="d8395-154">Örnekler için şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="d8395-154">For examples include:</span></span>
  * <span data-ttu-id="d8395-155">Sertifikanın hizmetlerinize göre bilinip tanınmadığını belirleme.</span><span class="sxs-lookup"><span data-stu-id="d8395-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="d8395-156">Kendi sorumlunuzu oluşturma.</span><span class="sxs-lookup"><span data-stu-id="d8395-156">Constructing your own principal.</span></span> <span data-ttu-id="d8395-157">`Startup.ConfigureServices`içinde aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d8395-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="d8395-158">Gelen sertifikayı, ek doğrulamadan uymadığını fark ederseniz bir hata nedeniyle `context.Fail("failure reason")` çağırın.</span><span class="sxs-lookup"><span data-stu-id="d8395-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="d8395-159">Gerçek işlevsellik için muhtemelen bir veritabanına veya diğer Kullanıcı Mağazası türüne bağlanan bağımlılık ekleme bölümünde kayıtlı bir hizmeti çağırmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="d8395-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="d8395-160">Temsilciniz 'e geçirilen bağlamı kullanarak hizmetinize erişin.</span><span class="sxs-lookup"><span data-stu-id="d8395-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="d8395-161">`Startup.ConfigureServices`içinde aşağıdaki örneği göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d8395-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="d8395-162">Kavramsal olarak, sertifikanın doğrulanması bir yetkilendirme konusudur.</span><span class="sxs-lookup"><span data-stu-id="d8395-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="d8395-163">`OnCertificateValidated`içinde değil, yetkilendirme ilkesindeki bir veren veya parmak izi gibi bir denetim eklemek, kusursuz kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="d8395-164">Ana bilgisayarınızı sertifika gerektirecek şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d8395-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="d8395-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d8395-165">Kestrel</span></span>

<span data-ttu-id="d8395-166">*Program.cs*' de, Kestrel ' yi aşağıdaki şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="d8395-166">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="d8395-167">IIS</span><span class="sxs-lookup"><span data-stu-id="d8395-167">IIS</span></span>

<span data-ttu-id="d8395-168">IIS Yöneticisi 'nde aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="d8395-168">Complete the following steps in IIS Manager:</span></span>

1. <span data-ttu-id="d8395-169">**Bağlantılar** sekmesinden sitenizi seçin.</span><span class="sxs-lookup"><span data-stu-id="d8395-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="d8395-170">**Özellikler Görünümü** penceresinde **SSL ayarları** seçeneğine çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="d8395-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="d8395-171">**SSL gerektir** onay kutusunu Işaretleyin ve **istemci sertifikaları** bölümünde radyo **iste** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d8395-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![IIS 'de istemci sertifikası ayarları](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="d8395-173">Azure ve özel Web proxy 'leri</span><span class="sxs-lookup"><span data-stu-id="d8395-173">Azure and custom web proxies</span></span>

<span data-ttu-id="d8395-174">Sertifika iletme ara yazılımını yapılandırma hakkında bilgi için bkz. [konak ve dağıtım belgeleri](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) .</span><span class="sxs-lookup"><span data-stu-id="d8395-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>

### <a name="use-certificate-authentication-in-azure-web-apps"></a><span data-ttu-id="d8395-175">Azure Web Apps sertifika kimlik doğrulamasını kullanma</span><span class="sxs-lookup"><span data-stu-id="d8395-175">Use certificate authentication in Azure Web Apps</span></span>

<span data-ttu-id="d8395-176">`AddCertificateForwarding` yöntemi şunu belirtmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="d8395-176">The `AddCertificateForwarding` method is used to specify:</span></span>

* <span data-ttu-id="d8395-177">İstemci üst bilgi adı.</span><span class="sxs-lookup"><span data-stu-id="d8395-177">The client header name.</span></span>
* <span data-ttu-id="d8395-178">Sertifika nasıl yüklenir (`HeaderConverter` özelliği kullanılarak).</span><span class="sxs-lookup"><span data-stu-id="d8395-178">How the certificate is to be loaded (using the `HeaderConverter` property).</span></span>

<span data-ttu-id="d8395-179">Azure Web Apps, sertifika, `X-ARR-ClientCert`adlı özel bir istek üst bilgisi olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-179">In Azure Web Apps, the certificate is passed as a custom request header named `X-ARR-ClientCert`.</span></span> <span data-ttu-id="d8395-180">Bunu kullanmak için `Startup.ConfigureServices`sertifika iletmeyi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="d8395-180">To use it, configure certificate forwarding in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddCertificateForwarding(options =>
{
    options.CertificateHeader = "X-ARR-ClientCert";
    options.HeaderConverter = (headerValue) =>
    {
        X509Certificate2 clientCertificate = null;
        if(!string.IsNullOrWhiteSpace(headerValue))
        {
            byte[] bytes = StringToByteArray(headerValue);
            clientCertificate = new X509Certificate2(bytes);
        }

        return clientCertificate;
    };
});
```

<span data-ttu-id="d8395-181">`Startup.Configure` yöntemi daha sonra ara yazılımı ekler.</span><span class="sxs-lookup"><span data-stu-id="d8395-181">The `Startup.Configure` method then adds the middleware.</span></span> <span data-ttu-id="d8395-182">`UseCertificateForwarding`, `UseAuthentication` ve `UseAuthorization`çağrılarından önce çağrılır:</span><span class="sxs-lookup"><span data-stu-id="d8395-182">`UseCertificateForwarding` is called before the calls to `UseAuthentication` and `UseAuthorization`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...
    
    app.UseRouting();

    app.UseCertificateForwarding();
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

<span data-ttu-id="d8395-183">Ayrı bir sınıf, doğrulama mantığını uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-183">A separate class can be used to implement validation logic.</span></span> <span data-ttu-id="d8395-184">Bu örnekte otomatik olarak imzalanan sertifika kullanıldığından, yalnızca sertifikanızın kullanılabildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="d8395-184">Because the same self-signed certificate is used in this example, ensure that only your certificate can be used.</span></span> <span data-ttu-id="d8395-185">İstemci sertifikasının ve sunucu sertifikasının parmak izlerinin eşleştiğinden emin olun, aksi takdirde herhangi bir sertifika kullanılabilir ve kimlik doğrulaması için yeterli olacaktır.</span><span class="sxs-lookup"><span data-stu-id="d8395-185">Validate that the thumbprints of both the client certificate and the server certificate match, otherwise any certificate can be used and will be enough to authenticate.</span></span> <span data-ttu-id="d8395-186">Bu, `AddCertificate` yöntemi içinde kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="d8395-186">This would be used inside the `AddCertificate` method.</span></span> <span data-ttu-id="d8395-187">Ara veya alt sertifikalar kullanıyorsanız konuyu veya sertifikayı vereni de doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d8395-187">You could also validate the subject or the issuer here if you're using intermediate or child certificates.</span></span>

```csharp
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            // Do not hardcode passwords in production code, use thumbprint or key vault
            var cert = new X509Certificate2(Path.Combine("sts_dev_cert.pfx"), "1234");
            if (clientCertificate.Thumbprint == cert.Thumbprint)
            {
                return true;
            }

            return false;
        }
    }
}
```

#### <a name="implement-an-httpclient-using-a-certificate"></a><span data-ttu-id="d8395-188">Sertifika kullanarak bir HttpClient uygulama</span><span class="sxs-lookup"><span data-stu-id="d8395-188">Implement an HttpClient using a certificate</span></span>

<span data-ttu-id="d8395-189">Web API istemcisi, bir `IHttpClientFactory` örneği kullanılarak oluşturulan bir `HttpClient`kullanır.</span><span class="sxs-lookup"><span data-stu-id="d8395-189">The web API client uses an `HttpClient`, which was created using an `IHttpClientFactory` instance.</span></span> <span data-ttu-id="d8395-190">Bu, `HttpClient`için bir işleyici tanımlamak için bir yol sağlamaz, bu nedenle sertifikayı `X-ARR-ClientCert` istek başlığına eklemek için bir `HttpRequestMessage` kullanın.</span><span class="sxs-lookup"><span data-stu-id="d8395-190">This doesn't provide a way to define a handler for the `HttpClient`, so use an `HttpRequestMessage` to add the certificate to the `X-ARR-ClientCert` request header.</span></span> <span data-ttu-id="d8395-191">Sertifika, `GetRawCertDataString` yöntemi kullanılarak bir dize olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="d8395-191">The certificate is added as a string using the `GetRawCertDataString` method.</span></span> 

```csharp
private async Task<JsonDocument> GetApiDataAsync()
{
    try
    {
        // Do not hardcode passwords in production code, use thumbprint or key vault
        var cert = new X509Certificate2(Path.Combine(_environment.ContentRootPath, "sts_dev_cert.pfx"), "1234");

        var client = _clientFactory.CreateClient();

        var request = new HttpRequestMessage()
        {
            RequestUri = new Uri("https://localhost:44379/api/values"),
            Method = HttpMethod.Get,
        };

        request.Headers.Add("X-ARR-ClientCert", cert.GetRawCertDataString());
        var response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            var responseContent = await response.Content.ReadAsStringAsync();
            var data = JsonDocument.Parse(responseContent);

            return data;
        }

        throw new ApplicationException($"Status code: {response.StatusCode}, Error: {response.ReasonPhrase}");
    }
    catch (Exception e)
    {
        throw new ApplicationException($"Exception {e}");
    }
}
```

<span data-ttu-id="d8395-192">Sunucuya doğru sertifika gönderiliyorsa, veriler döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d8395-192">If the correct certificate is sent to the server, the data is returned.</span></span> <span data-ttu-id="d8395-193">Sertifika yoksa veya yanlış sertifika gönderilmezse bir HTTP 403 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d8395-193">If no certificate or the wrong certificate is sent, an HTTP 403 status code is returned.</span></span>

### <a name="create-certificates-in-powershell"></a><span data-ttu-id="d8395-194">PowerShell 'de sertifika oluşturma</span><span class="sxs-lookup"><span data-stu-id="d8395-194">Create certificates in PowerShell</span></span>

<span data-ttu-id="d8395-195">Sertifikaların oluşturulması, bu akışı ayarlamanın en zor bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="d8395-195">Creating the certificates is the hardest part in setting up this flow.</span></span> <span data-ttu-id="d8395-196">Bir kök sertifika `New-SelfSignedCertificate` PowerShell cmdlet 'i kullanılarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-196">A root certificate can be created using the `New-SelfSignedCertificate` PowerShell cmdlet.</span></span> <span data-ttu-id="d8395-197">Sertifikayı oluştururken güçlü bir parola kullanın.</span><span class="sxs-lookup"><span data-stu-id="d8395-197">When creating the certificate, use a strong password.</span></span> <span data-ttu-id="d8395-198">`KeyUsageProperty` parametresini ve `KeyUsage` parametresini gösterildiği gibi eklemek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="d8395-198">It's important to add the `KeyUsageProperty` parameter and the `KeyUsage` parameter as shown.</span></span>

#### <a name="create-root-ca"></a><span data-ttu-id="d8395-199">Kök CA oluştur</span><span class="sxs-lookup"><span data-stu-id="d8395-199">Create root CA</span></span>

```powershell
New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath root_ca_dev_damienbod.crt
```

#### <a name="install-in-the-trusted-root"></a><span data-ttu-id="d8395-200">Güvenilen köke yüklensin</span><span class="sxs-lookup"><span data-stu-id="d8395-200">Install in the trusted root</span></span>

<span data-ttu-id="d8395-201">Kök sertifikanın ana bilgisayar sisteminizde güvenilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d8395-201">The root certificate needs to be trusted on your host system.</span></span> <span data-ttu-id="d8395-202">Sertifika yetkilisi tarafından oluşturulmamış bir kök sertifikaya varsayılan olarak güvenilmez.</span><span class="sxs-lookup"><span data-stu-id="d8395-202">A root certificate which was not created by a certificate authority won't be trusted by default.</span></span> <span data-ttu-id="d8395-203">Aşağıdaki bağlantıda bunun Windows 'da nasıl gerçekleştirebileceği açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="d8395-203">The following link explains how this can be accomplished on Windows:</span></span>

https://social.msdn.microsoft.com/Forums/SqlServer/5ed119ef-1704-4be4-8a4f-ef11de7c8f34/a-certificate-chain-processed-but-terminated-in-a-root-certificate-which-is-not-trusted-by-the

#### <a name="intermediate-certificate"></a><span data-ttu-id="d8395-204">Ara sertifika</span><span class="sxs-lookup"><span data-stu-id="d8395-204">Intermediate certificate</span></span>

<span data-ttu-id="d8395-205">Artık kök sertifikadan bir ara sertifika oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-205">An intermediate certificate can now be created from the root certificate.</span></span> <span data-ttu-id="d8395-206">Bu, tüm kullanım durumları için gerekli değildir, ancak birçok sertifika oluşturmanız veya sertifika gruplarını etkinleştirmeniz veya devre dışı bırakmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-206">This isn't required for all use cases, but you might need to create many certificates or need to activate or disable groups of certificates.</span></span> <span data-ttu-id="d8395-207">Sertifikanın temel kısıtlamalarında yol uzunluğunu ayarlamak için `TextExtension` parametresi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d8395-207">The `TextExtension` parameter is required to set the path length in the basic constraints of the certificate.</span></span>

<span data-ttu-id="d8395-208">Ara sertifika daha sonra Windows ana bilgisayar sistemindeki güvenilen ara sertifikaya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-208">The intermediate certificate can then be added to the trusted intermediate certificate in the Windows host system.</span></span>

```powershell
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint of the root..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "intermediate_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "intermediate_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\intermediate_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath intermediate_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-intermediate-certificate"></a><span data-ttu-id="d8395-209">Ara sertifikadan alt sertifika oluştur</span><span class="sxs-lookup"><span data-stu-id="d8395-209">Create child certificate from intermediate certificate</span></span>

<span data-ttu-id="d8395-210">Ara sertifikadan bir alt sertifika oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-210">A child certificate can be created from the intermediate certificate.</span></span> <span data-ttu-id="d8395-211">Bu, son varlıktır ve daha fazla alt sertifika oluşturmak zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="d8395-211">This is the end entity and doesn't need to create more child certificates.</span></span>

```powershell
$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the Intermediate certificate..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-root-certificate"></a><span data-ttu-id="d8395-212">Kök sertifikadan alt sertifika oluştur</span><span class="sxs-lookup"><span data-stu-id="d8395-212">Create child certificate from root certificate</span></span>

<span data-ttu-id="d8395-213">Kök sertifikadan doğrudan bir alt sertifika da oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-213">A child certificate can also be created from the root certificate directly.</span></span> 

```powershell
$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the root cert..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="example-root---intermediate-certificate---certificate"></a><span data-ttu-id="d8395-214">Örnek kök-ara sertifika-sertifika</span><span class="sxs-lookup"><span data-stu-id="d8395-214">Example root - intermediate certificate - certificate</span></span>

```powershell
$mypwdroot = ConvertTo-SecureString -String "1234" -Force -AsPlainText
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

Get-ChildItem -Path cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwdroot

Export-Certificate -Cert cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 -FilePath root_ca_dev_damienbod.crt

$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\0C89639E4E2998A93E423F919B36D4009A0F9991 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 -FilePath child_a_dev_damienbod.crt

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\BA9BF91ED35538A01375EFC212A2F46104B33A44 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_b_from_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_b_from_a_dev_damienbod.com" 

Get-ChildItem -Path cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_b_from_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A -FilePath child_b_from_a_dev_damienbod.crt
```

<span data-ttu-id="d8395-215">Kök, ara veya alt sertifikaları kullanırken, sertifikalar veren veya özne kullanılarak doğrulanabilir.</span><span class="sxs-lookup"><span data-stu-id="d8395-215">When using the root, intermediate, or child certificates, the certificates can be validated using the Issuer or the Subject as required.</span></span>

```csharp
using System.Collections.Generic;
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService 
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            return CheckIfThumbprintIsValid(clientCertificate);
        }

        private bool CheckIfThumbprintIsValid(X509Certificate2 clientCertificate)
        {
            var listOfValidThumbprints = new List<string>
            {
                "141594A0AE38CBBECED7AF680F7945CD51D8F28A",
                "0C89639E4E2998A93E423F919B36D4009A0F9991",
                "BA9BF91ED35538A01375EFC212A2F46104B33A44"
            };

            if (listOfValidThumbprints.Contains(clientCertificate.Thumbprint))
            {
                return true;
            }

            return false;
        }
    }
}
```
