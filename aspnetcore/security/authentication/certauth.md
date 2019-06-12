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
# <a name="overview"></a><span data-ttu-id="00eda-103">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="00eda-103">Overview</span></span>

<span data-ttu-id="00eda-104">`Microsoft.AspNetCore.Authentication.Certificate` benzer şekilde bir uygulaması içeren [sertifika kimlik doğrulaması](https://tools.ietf.org/html/rfc5246#section-7.4.4) ASP.NET Core için.</span><span class="sxs-lookup"><span data-stu-id="00eda-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="00eda-105">Şimdiye kadar bir ASP.NET Core alır önce sertifika kimlik doğrulaması TLS düzeyinde, uzun'olmuyor.</span><span class="sxs-lookup"><span data-stu-id="00eda-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="00eda-106">Bu sertifikayı doğrular ve ardından, nerede, giderebilir Bu sertifika için bir olay verir. bir kimlik doğrulama işleyicisi daha doğru bir şekilde, bir `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="00eda-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="00eda-107">[Ana yapılandırma](#configure-your-host-to-require-certificates) sertifika kimlik doğrulaması için IIS, Kestrel, Azure Web Apps veya başka bir runbook'tan kullanmakta olduğunuz olması.</span><span class="sxs-lookup"><span data-stu-id="00eda-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="00eda-108">Kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="00eda-108">Get started</span></span>

<span data-ttu-id="00eda-109">Bir HTTPS sertifikası alın, bunu, uygulama ve [ana yapılandırma](#configure-your-host-to-require-certificates) sertifikaları gerektirmek için.</span><span class="sxs-lookup"><span data-stu-id="00eda-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="00eda-110">Web uygulamanızda bir başvuru ekleyin `Microsoft.AspNetCore.Authentication.Certificate` paket.</span><span class="sxs-lookup"><span data-stu-id="00eda-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="00eda-111">Ardından `Startup.Configure` yöntemi, çağrı `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` ile seçeneklerinizi için temsilci sağlayarak `OnCertificateValidated` gönderdiği istekleri ile istemci sertifikası herhangi ek doğrulama yapmak için.</span><span class="sxs-lookup"><span data-stu-id="00eda-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="00eda-112">Bilgileri Aç bir `ClaimsPrincipal` ve açık ayarının `context.Principal` özelliği.</span><span class="sxs-lookup"><span data-stu-id="00eda-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="00eda-113">Kimlik doğrulama başarısız olursa, bu işleyicisini döndürür bir `403 (Forbidden)` yanıt yerine bir `401 (Unauthorized)`, bekleyebileceğiniz gibi.</span><span class="sxs-lookup"><span data-stu-id="00eda-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="00eda-114">Mantık, kimlik doğrulaması sırasında ilk TLS bağlantı olacağını ' dir.</span><span class="sxs-lookup"><span data-stu-id="00eda-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="00eda-115">İşleyici ulaştığında zamanına göre çok geç.</span><span class="sxs-lookup"><span data-stu-id="00eda-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="00eda-116">Bağlantı bir sertifikayla anonim bir bağlantıdan yükseltmek için hiçbir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="00eda-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="00eda-117">Ayrıca `app.UseAuthentication();` içinde `Startup.Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="00eda-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="00eda-118">Aksi takdirde, HttpContext.User ayarlanmaz `ClaimsPrincipal` sertifikadan oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="00eda-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="00eda-119">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="00eda-119">For example:</span></span>

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

<span data-ttu-id="00eda-120">Yukarıdaki örnekte, sertifika kimlik doğrulaması eklemek için varsayılan yolu gösterir.</span><span class="sxs-lookup"><span data-stu-id="00eda-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="00eda-121">Ortak sertifika özellikleri kullanarak bir kullanıcı asıl işleyicisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="00eda-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="00eda-122">Sertifika doğrulaması'nı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="00eda-122">Configure certificate validation</span></span>

<span data-ttu-id="00eda-123">`CertificateAuthenticationOptions` İşleyicisi sunucusundaki bir sertifikanın gerçekleştirmesi gereken en düşük doğrulamaları olan bazı yerleşik doğrulamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="00eda-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="00eda-124">Bu ayarların her biri varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="00eda-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="00eda-125">AllowedCertificateTypes = zincirleme, SelfSigned ya da tüm (zincirleme | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="00eda-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="00eda-126">Bu denetim yalnızca uygun sertifika türü izin verildiğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="00eda-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="00eda-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="00eda-127">ValidateCertificateUse</span></span>

<span data-ttu-id="00eda-128">Bu denetim, istemci tarafından sunulan sertifika istemci kimlik doğrulaması genişletilmiş anahtar kullanımı (EKU) veya EKU listelenmez hiç sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="00eda-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="00eda-129">EKU yok belirtilirse belirtimleri söyleyin gibi tüm EKU'larına geçerli sayılan.</span><span class="sxs-lookup"><span data-stu-id="00eda-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="00eda-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="00eda-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="00eda-131">Bu onay, sertifikanın geçerlilik süresi olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="00eda-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="00eda-132">Her bir istek üzerinde geçerli bir oturum sırasında bu iletildiğinde geçerli bir sertifika süresinin dolmadığından emin işleyici sağlar.</span><span class="sxs-lookup"><span data-stu-id="00eda-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="00eda-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="00eda-133">RevocationFlag</span></span>

<span data-ttu-id="00eda-134">Hangi sertifikaların zincirinde belirten bir bayrak iptali için denetlenir.</span><span class="sxs-lookup"><span data-stu-id="00eda-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="00eda-135">İptal denetimlerini yalnızca sertifikasının bir kök sertifikaya zincir gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="00eda-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="00eda-136">revocationMode</span><span class="sxs-lookup"><span data-stu-id="00eda-136">RevocationMode</span></span>

<span data-ttu-id="00eda-137">İptal denetimlerini nasıl gerçekleştirileceğini belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="00eda-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="00eda-138">Sertifika yetkilisi bağlantı kurulurken bir çevrimiçi denetimi belirtme uzun gecikmelere neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="00eda-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="00eda-139">İptal denetimlerini yalnızca sertifikasının bir kök sertifikaya zincir gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="00eda-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="00eda-140">Yalnızca belirli yollardaki bir sertifika istemek için uygulamamı yapılandırabilirim?</span><span class="sxs-lookup"><span data-stu-id="00eda-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="00eda-141">Bu mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="00eda-141">This isn't possible.</span></span> <span data-ttu-id="00eda-142">Bu nedenle hiçbir istek alanlara göre kapsam mümkün değildir, ilk isteği bu bağlantıda alınmadan önce HTTPS iletişiminin başlangıç, sunucu tarafından işlemin tamamlandığını sertifika exchange yapılır unutmayın.</span><span class="sxs-lookup"><span data-stu-id="00eda-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="00eda-143">İşleyici olayları</span><span class="sxs-lookup"><span data-stu-id="00eda-143">Handler events</span></span>

<span data-ttu-id="00eda-144">İki olay işleyicisi sahiptir:</span><span class="sxs-lookup"><span data-stu-id="00eda-144">The handler has two events:</span></span>

* <span data-ttu-id="00eda-145">`OnAuthenticationFailed` &ndash; Bir özel durum kimlik doğrulaması sırasında gerçekleşir ve react sağlar çağrılır.</span><span class="sxs-lookup"><span data-stu-id="00eda-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="00eda-146">`OnCertificateValidated` &ndash; Doğrulama başarılı oldu sertifika doğrulandıysa ve varsayılan sorumlu oluşturulduktan sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="00eda-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="00eda-147">Bu olay, kendi doğrulama işlemini yapabilir ve artırabilir veya asıl değiştirin olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="00eda-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="00eda-148">İçin örnekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="00eda-148">For examples include:</span></span>
  * <span data-ttu-id="00eda-149">Sertifika hizmetlerinize biliniyorsa belirleme.</span><span class="sxs-lookup"><span data-stu-id="00eda-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="00eda-150">Kendi asıl oluşturma.</span><span class="sxs-lookup"><span data-stu-id="00eda-150">Constructing your own principal.</span></span> <span data-ttu-id="00eda-151">Aşağıdaki örnekte göz önünde bulundurun `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="00eda-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="00eda-152">Gelen sertifika, ek doğrulama karşılamadığında bulursanız, çağrı `context.Fail("failure reason")` bir başarısızlık nedeniyle.</span><span class="sxs-lookup"><span data-stu-id="00eda-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="00eda-153">Gerçek işlevleri, büyük olasılıkla bir veritabanı veya başka bir kullanıcı deposu türü bağlayan bir bağımlılık ekleme kayıtlı bir hizmeti çağırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="00eda-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="00eda-154">Hizmetiniz, temsilciye iletilen bağlamını kullanarak erişin.</span><span class="sxs-lookup"><span data-stu-id="00eda-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="00eda-155">Aşağıdaki örnekte göz önünde bulundurun `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="00eda-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="00eda-156">Kavramsal olarak, doğrulama sertifikasının bir yetkilendirme konusudur.</span><span class="sxs-lookup"><span data-stu-id="00eda-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="00eda-157">Örneğin bir denetimi, ekleme, bir veren veya parmak izi bir yetkilendirme ilkesi, yerine iç `OnCertificateValidated`, mükemmel bir şekilde kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="00eda-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="00eda-158">Sertifikaları gerektirmek için ana yapılandırma</span><span class="sxs-lookup"><span data-stu-id="00eda-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="00eda-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="00eda-159">Kestrel</span></span>

<span data-ttu-id="00eda-160">İçinde *Program.cs*, Kestrel aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="00eda-160">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="00eda-161">IIS</span><span class="sxs-lookup"><span data-stu-id="00eda-161">IIS</span></span>

<span data-ttu-id="00eda-162">IIS Yöneticisi'nde aşağıdaki adımları tamamlayın:</span><span class="sxs-lookup"><span data-stu-id="00eda-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="00eda-163">Sitenizi seçin **bağlantıları** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="00eda-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="00eda-164">Çift **SSL ayarları** seçeneğini **özellikler görünümü** penceresi.</span><span class="sxs-lookup"><span data-stu-id="00eda-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="00eda-165">Denetleyin **SSL iste** onay kutusunu seçip **gerektiren** radyo düğmesinin **istemci sertifikaları** bölümü.</span><span class="sxs-lookup"><span data-stu-id="00eda-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![IIS istemci sertifika ayarları](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="00eda-167">Azure ve özel web Ara sunucuları</span><span class="sxs-lookup"><span data-stu-id="00eda-167">Azure and custom web proxies</span></span>

<span data-ttu-id="00eda-168">Bkz: [barındırma ve dağıtma belgeleri](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) ara yazılım iletme sertifikası yapılandırma için.</span><span class="sxs-lookup"><span data-stu-id="00eda-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
