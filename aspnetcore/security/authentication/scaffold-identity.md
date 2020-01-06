---
title: ASP.NET Core projelerinde yapı iskelesi kimliği
author: rick-anderson
description: ASP.NET Core projesindeki kimliği nasıl yapılandıracağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 2432d346d9678157848a38fa01d9057cdd7503ff
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75356282"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="49042-103">ASP.NET Core projelerinde yapı iskelesi kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="49042-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49042-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="49042-105">ASP.NET Core [Razor sınıfı kitaplığı](xref:razor-pages/ui-class)olarak [ASP.NET Core kimliği](xref:security/authentication/identity) sağlar.</span><span class="sxs-lookup"><span data-stu-id="49042-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="49042-106">Kimlik içeren uygulamalar, Identity Razor sınıf kitaplığı 'nda (RCL) bulunan kaynak kodu seçmeli olarak eklemek için desteği 'ı uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="49042-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="49042-107">Kodu değiştirebilmeniz ve davranışı değiştirebilmek için kaynak kodu oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="49042-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="49042-108">Örneğin, kayıt sırasında kullanılan kodu oluşturmak için desteği ' ı söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="49042-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="49042-109">Oluşturulan kod, RCL kimliği içindeki aynı koddan önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="49042-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="49042-110">Kullanıcı arabirimine tam denetim sağlamak ve varsayılan RCL 'yi kullanmak için, [tam KIMLIK UI kaynağı oluşturma](#full)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="49042-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="49042-111">Kimlik **doğrulaması içermeyen uygulamalar** , RCL kimlik paketini eklemek için desteği uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="49042-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="49042-112">Oluşturulacak kimlik kodunu seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="49042-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="49042-113">Desteği gerekli kodların çoğunu üretse de, işlemi gerçekleştirmek için projenizi güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="49042-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="49042-114">Bu belgede, bir kimlik yapı iskelesi güncelleştirmesini tamamlaması için gereken adımlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="49042-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="49042-115">Dosya farklılıklarını gösteren ve değişikliklerden geri dönüş yapmanızı sağlayan bir kaynak denetimi sistemi kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="49042-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="49042-116">Kimlik scaffolder 'ı çalıştırdıktan sonra değişiklikleri inceleyin.</span><span class="sxs-lookup"><span data-stu-id="49042-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="49042-117">[Iki öğeli kimlik doğrulaması](xref:security/authentication/identity-enable-qrcodes), [Hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)ve kimlik ile diğer güvenlik özellikleri kullanılırken hizmetler gereklidir.</span><span class="sxs-lookup"><span data-stu-id="49042-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="49042-118">Hizmet veya hizmet saplamaları, kimliğe iskele oluştururken oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="49042-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="49042-119">Bu özelliklerin etkinleştirilmesi için hizmetlerin el ile eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="49042-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="49042-120">Örneğin, bkz. [e-posta onayı gerektir](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="49042-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="49042-121">Bu belgede, scaffolder çalıştırılırken oluşturulan *Scaffoldingreadme. txt* dosyasından daha eksiksiz yönergeler bulunur.</span><span class="sxs-lookup"><span data-stu-id="49042-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="49042-122">Boş bir projede yapı iskelesi kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49042-123">`Startup` sınıfını aşağıdakine benzer kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="49042-124">Mevcut yetkilendirme olmadan bir Razor projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-124">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49042-125">Kimlik, */kimlik/ıdentityhostingstartup. cs alanlarında*yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49042-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49042-126">daha fazla bilgi için bkz. [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="49042-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="49042-127">Geçişler, UseAuthentication ve düzen</span><span class="sxs-lookup"><span data-stu-id="49042-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="49042-128">Kimlik doğrulamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="49042-128">Enable authentication</span></span>

<span data-ttu-id="49042-129">`Startup` sınıfını aşağıdakine benzer kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="49042-130">Düzen değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="49042-130">Layout changes</span></span>

<span data-ttu-id="49042-131">İsteğe bağlı: (`_LoginPartial`) oturum açma bölümünü düzen dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="49042-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="49042-132">Yetkilendirmeyi içeren bir Razor projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-132">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="49042-133">Bazı kimlik seçenekleri */Identity/ıdentityhostingstartup. cs alanlarında*yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49042-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49042-134">Daha fazla bilgi için bkz. [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="49042-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="49042-135">Var olan yetkilendirme olmadan bir MVC projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-135">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49042-136">İsteğe bağlı: (`_LoginPartial`) öğesini *Görünümler/Shared/_Layout. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="49042-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="49042-137">*Pages/Shared/_LoginPartial. cshtml* dosyasını *views/Shared/_LoginPartial. cshtml* olarak taşıyın</span><span class="sxs-lookup"><span data-stu-id="49042-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="49042-138">Kimlik, */kimlik/ıdentityhostingstartup. cs alanlarında*yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49042-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49042-139">Daha fazla bilgi için bkz. ıhostingstartup.</span><span class="sxs-lookup"><span data-stu-id="49042-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="49042-140">`Startup` sınıfını aşağıdakine benzer kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="49042-141">Yetkilendirmeyi içeren bir MVC projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="49042-142">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="49042-142">Create full identity UI source</span></span>

<span data-ttu-id="49042-143">Kimlik Kullanıcı arabirimine tam denetim sağlamak için, Identity desteği ' ı çalıştırın ve **tüm dosyaları geçersiz kıl**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="49042-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="49042-144">Aşağıdaki Vurgulanan kodda, varsayılan kimlik Kullanıcı arabirimini ASP.NET Core 2,1 Web uygulamasındaki kimlikle değiştirme değişiklikleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="49042-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="49042-145">Bunu, kimlik Kullanıcı arabirimine tam denetim sağlamak için yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="49042-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="49042-146">Varsayılan kimlik aşağıdaki kodda değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="49042-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="49042-147">Aşağıdaki kod, [Loginpath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [Logoutpath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)ve [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)öğesini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="49042-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="49042-148">Bir `IEmailSender` uygulamasını kaydedin, örneğin:</span><span class="sxs-lookup"><span data-stu-id="49042-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="49042-149">Kayıt sayfasını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="49042-149">Disable register page</span></span>

<span data-ttu-id="49042-150">Kullanıcı kaydını devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="49042-150">To disable user registration:</span></span>

* <span data-ttu-id="49042-151">Yapı iskelesi kimliği.</span><span class="sxs-lookup"><span data-stu-id="49042-151">Scaffold Identity.</span></span> <span data-ttu-id="49042-152">Account. Register, Account. Login ve account. RegisterConfirmation bilgilerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="49042-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="49042-153">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="49042-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="49042-154">Kullanıcıları bu uç noktadan kaydedememesi için *alan/kimlik/sayfa/hesap/kayıt. cshtml. cs* 'yi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="49042-155">Önceki değişikliklerle tutarlı olması için *alan/kimlik/sayfa/hesap/kayıt. cshtml* 'yi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="49042-156">*Alan/kimlik/sayfa/hesap/Login. cshtml* 'den kayıt bağlantısını açıklama veya kaldırma</span><span class="sxs-lookup"><span data-stu-id="49042-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="49042-157">*Bölgeler/kimlik/sayfalar/hesap/RegisterConfirmation* sayfasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="49042-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="49042-158">Cshtml dosyasındaki kodu ve bağlantıları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="49042-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="49042-159">`PageModel`onay kodunu kaldırın:</span><span class="sxs-lookup"><span data-stu-id="49042-159">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="49042-160">Kullanıcıları eklemek için başka bir uygulama kullanma</span><span class="sxs-lookup"><span data-stu-id="49042-160">Use another app to add users</span></span>

<span data-ttu-id="49042-161">Web uygulaması dışındaki kullanıcıları eklemek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="49042-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="49042-162">Kullanıcı ekleme seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="49042-162">Options to add users include:</span></span>

* <span data-ttu-id="49042-163">Adanmış bir yönetim Web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="49042-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="49042-164">Bir konsol uygulaması.</span><span class="sxs-lookup"><span data-stu-id="49042-164">A console app.</span></span>

<span data-ttu-id="49042-165">Aşağıdaki kod, kullanıcı eklemeye yönelik bir yaklaşımı özetler:</span><span class="sxs-lookup"><span data-stu-id="49042-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="49042-166">Kullanıcıların listesi belleği okur.</span><span class="sxs-lookup"><span data-stu-id="49042-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="49042-167">Her Kullanıcı için güçlü bir benzersiz parola oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="49042-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="49042-168">Kullanıcı, kimlik veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="49042-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="49042-169">Kullanıcıya bildirilir ve parolayı değiştirmesi bildirilir.</span><span class="sxs-lookup"><span data-stu-id="49042-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="49042-170">Aşağıdaki kod, bir kullanıcı ekleme ana hatlarıyla verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="49042-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="49042-171">Üretim senaryolarında de benzer bir yaklaşım izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="49042-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49042-172">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="49042-172">Additional resources</span></span>

* [<span data-ttu-id="49042-173">ASP.NET Core 2,1 ve üzeri kimlik doğrulama kodundaki değişiklikler</span><span class="sxs-lookup"><span data-stu-id="49042-173">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="49042-174">ASP.NET Core 2,1 ve üzeri, [Razor sınıf kitaplığı](xref:razor-pages/ui-class)olarak [ASP.NET Core kimliği](xref:security/authentication/identity) sağlar.</span><span class="sxs-lookup"><span data-stu-id="49042-174">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="49042-175">Kimlik içeren uygulamalar, Identity Razor sınıf kitaplığı 'nda (RCL) bulunan kaynak kodu seçmeli olarak eklemek için desteği 'ı uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="49042-175">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="49042-176">Kodu değiştirebilmeniz ve davranışı değiştirebilmek için kaynak kodu oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="49042-176">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="49042-177">Örneğin, kayıt sırasında kullanılan kodu oluşturmak için desteği ' ı söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="49042-177">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="49042-178">Oluşturulan kod, RCL kimliği içindeki aynı koddan önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="49042-178">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="49042-179">Kullanıcı arabirimine tam denetim sağlamak ve varsayılan RCL 'yi kullanmak için, [tam KIMLIK UI kaynağı oluşturma](#full)bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="49042-179">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="49042-180">Kimlik **doğrulaması içermeyen uygulamalar** , RCL kimlik paketini eklemek için desteği uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="49042-180">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="49042-181">Oluşturulacak kimlik kodunu seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="49042-181">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="49042-182">Desteği gerekli kodların çoğunu üretse de, işlemi gerçekleştirmek için projenizi güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="49042-182">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="49042-183">Bu belgede, bir kimlik yapı iskelesi güncelleştirmesini tamamlaması için gereken adımlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="49042-183">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="49042-184">Identity desteği çalıştırıldığında, proje dizininde bir *scaffoldingreadme. txt* dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="49042-184">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="49042-185">*Scaffoldingreadme. txt* dosyası, kimlik yapı iskelesi güncelleştirmesinin tamamlanabilmesi için gerekli olan genel yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="49042-185">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="49042-186">Bu belge, *Scaffoldingreadme. txt* dosyasından daha eksiksiz yönergeler içerir.</span><span class="sxs-lookup"><span data-stu-id="49042-186">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="49042-187">Dosya farklılıklarını gösteren ve değişikliklerden geri dönüş yapmanızı sağlayan bir kaynak denetimi sistemi kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="49042-187">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="49042-188">Kimlik scaffolder 'ı çalıştırdıktan sonra değişiklikleri inceleyin.</span><span class="sxs-lookup"><span data-stu-id="49042-188">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="49042-189">[Iki öğeli kimlik doğrulaması](xref:security/authentication/identity-enable-qrcodes), [Hesap onaylama ve parola kurtarma](xref:security/authentication/accconfirm)ve kimlik ile diğer güvenlik özellikleri kullanılırken hizmetler gereklidir.</span><span class="sxs-lookup"><span data-stu-id="49042-189">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="49042-190">Hizmet veya hizmet saplamaları, kimliğe iskele oluştururken oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="49042-190">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="49042-191">Bu özelliklerin etkinleştirilmesi için hizmetlerin el ile eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="49042-191">Services to enable these features must be added manually.</span></span> <span data-ttu-id="49042-192">Örneğin, bkz. [e-posta onayı gerektir](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="49042-192">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="49042-193">Boş bir projede yapı iskelesi kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-193">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49042-194">Aşağıdaki Vurgulanan çağrıları `Startup` sınıfına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="49042-194">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="49042-195">Mevcut yetkilendirme olmadan bir Razor projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-195">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49042-196">Kimlik, */kimlik/ıdentityhostingstartup. cs alanlarında*yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49042-196">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49042-197">daha fazla bilgi için bkz. [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="49042-197">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="49042-198">Geçişler, UseAuthentication ve düzen</span><span class="sxs-lookup"><span data-stu-id="49042-198">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="49042-199">Kimlik doğrulamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="49042-199">Enable authentication</span></span>

<span data-ttu-id="49042-200">`Startup` sınıfının `Configure` yönteminde `UseStaticFiles`sonra [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ' ı çağırın:</span><span class="sxs-lookup"><span data-stu-id="49042-200">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="49042-201">Düzen değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="49042-201">Layout changes</span></span>

<span data-ttu-id="49042-202">İsteğe bağlı: (`_LoginPartial`) oturum açma bölümünü düzen dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="49042-202">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="49042-203">Yetkilendirmeyi içeren bir Razor projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-203">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="49042-204">Bazı kimlik seçenekleri */Identity/ıdentityhostingstartup. cs alanlarında*yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49042-204">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49042-205">Daha fazla bilgi için bkz. [ıhostingstartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="49042-205">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="49042-206">Var olan yetkilendirme olmadan bir MVC projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-206">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="49042-207">İsteğe bağlı: (`_LoginPartial`) öğesini *Görünümler/Shared/_Layout. cshtml* dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="49042-207">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="49042-208">*Pages/Shared/_LoginPartial. cshtml* dosyasını *views/Shared/_LoginPartial. cshtml* olarak taşıyın</span><span class="sxs-lookup"><span data-stu-id="49042-208">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="49042-209">Kimlik, */kimlik/ıdentityhostingstartup. cs alanlarında*yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="49042-209">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="49042-210">Daha fazla bilgi için bkz. ıhostingstartup.</span><span class="sxs-lookup"><span data-stu-id="49042-210">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="49042-211">`UseStaticFiles`sonra [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 'ı çağır:</span><span class="sxs-lookup"><span data-stu-id="49042-211">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="49042-212">Yetkilendirmeyi içeren bir MVC projesinde kimlik oluşturma kimliği</span><span class="sxs-lookup"><span data-stu-id="49042-212">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="49042-213">*Sayfaları/paylaşılan* klasörünü ve bu klasördeki dosyaları silin.</span><span class="sxs-lookup"><span data-stu-id="49042-213">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="49042-214">Tam kimlik UI kaynağı oluşturma</span><span class="sxs-lookup"><span data-stu-id="49042-214">Create full identity UI source</span></span>

<span data-ttu-id="49042-215">Kimlik Kullanıcı arabirimine tam denetim sağlamak için, Identity desteği ' ı çalıştırın ve **tüm dosyaları geçersiz kıl**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="49042-215">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="49042-216">Aşağıdaki Vurgulanan kodda, varsayılan kimlik Kullanıcı arabirimini ASP.NET Core 2,1 Web uygulamasındaki kimlikle değiştirme değişiklikleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="49042-216">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="49042-217">Bunu, kimlik Kullanıcı arabirimine tam denetim sağlamak için yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="49042-217">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="49042-218">Varsayılan kimlik aşağıdaki kodda değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="49042-218">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="49042-219">Aşağıdaki kod, [Loginpath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [Logoutpath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)ve [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)öğesini ayarlar:</span><span class="sxs-lookup"><span data-stu-id="49042-219">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="49042-220">Bir `IEmailSender` uygulamasını kaydedin, örneğin:</span><span class="sxs-lookup"><span data-stu-id="49042-220">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="49042-221">Kayıt sayfasını devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="49042-221">Disable register page</span></span>

<span data-ttu-id="49042-222">Kullanıcı kaydını devre dışı bırakmak için:</span><span class="sxs-lookup"><span data-stu-id="49042-222">To disable user registration:</span></span>

* <span data-ttu-id="49042-223">Yapı iskelesi kimliği.</span><span class="sxs-lookup"><span data-stu-id="49042-223">Scaffold Identity.</span></span> <span data-ttu-id="49042-224">Account. Register, Account. Login ve account. RegisterConfirmation bilgilerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="49042-224">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="49042-225">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="49042-225">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="49042-226">Kullanıcıları bu uç noktadan kaydedememesi için *alan/kimlik/sayfa/hesap/kayıt. cshtml. cs* 'yi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-226">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="49042-227">Önceki değişikliklerle tutarlı olması için *alan/kimlik/sayfa/hesap/kayıt. cshtml* 'yi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="49042-227">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="49042-228">*Alan/kimlik/sayfa/hesap/Login. cshtml* 'den kayıt bağlantısını açıklama veya kaldırma</span><span class="sxs-lookup"><span data-stu-id="49042-228">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="49042-229">*Bölgeler/kimlik/sayfalar/hesap/RegisterConfirmation* sayfasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="49042-229">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="49042-230">Cshtml dosyasındaki kodu ve bağlantıları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="49042-230">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="49042-231">`PageModel`onay kodunu kaldırın:</span><span class="sxs-lookup"><span data-stu-id="49042-231">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="49042-232">Kullanıcıları eklemek için başka bir uygulama kullanma</span><span class="sxs-lookup"><span data-stu-id="49042-232">Use another app to add users</span></span>

<span data-ttu-id="49042-233">Web uygulaması dışındaki kullanıcıları eklemek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="49042-233">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="49042-234">Kullanıcı ekleme seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="49042-234">Options to add users include:</span></span>

* <span data-ttu-id="49042-235">Adanmış bir yönetim Web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="49042-235">A dedicated admin web app.</span></span>
* <span data-ttu-id="49042-236">Bir konsol uygulaması.</span><span class="sxs-lookup"><span data-stu-id="49042-236">A console app.</span></span>

<span data-ttu-id="49042-237">Aşağıdaki kod, kullanıcı eklemeye yönelik bir yaklaşımı özetler:</span><span class="sxs-lookup"><span data-stu-id="49042-237">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="49042-238">Kullanıcıların listesi belleği okur.</span><span class="sxs-lookup"><span data-stu-id="49042-238">A list of users is read into memory.</span></span>
* <span data-ttu-id="49042-239">Her Kullanıcı için güçlü bir benzersiz parola oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="49042-239">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="49042-240">Kullanıcı, kimlik veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="49042-240">The user is added to the Identity database.</span></span>
* <span data-ttu-id="49042-241">Kullanıcıya bildirilir ve parolayı değiştirmesi bildirilir.</span><span class="sxs-lookup"><span data-stu-id="49042-241">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="49042-242">Aşağıdaki kod, bir kullanıcı ekleme ana hatlarıyla verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="49042-242">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="49042-243">Üretim senaryolarında de benzer bir yaklaşım izlenebilir.</span><span class="sxs-lookup"><span data-stu-id="49042-243">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49042-244">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="49042-244">Additional resources</span></span>

* [<span data-ttu-id="49042-245">ASP.NET Core 2,1 ve üzeri kimlik doğrulama kodundaki değişiklikler</span><span class="sxs-lookup"><span data-stu-id="49042-245">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end