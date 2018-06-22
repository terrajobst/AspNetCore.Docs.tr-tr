---
title: ASP.NET Core projelerinde iskele kimliği
author: rick-anderson
description: ASP.NET Core projesinde kimlik iskele öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276324"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="a3186-103">ASP.NET Core projelerinde iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="a3186-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="a3186-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a3186-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a3186-105">ASP.NET Core 2.1 ve üzeri sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a3186-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a3186-106">Kimliği içeren uygulamaları kimlik Razor sınıf kitaplığı (RCL) bulunan kaynak koduna seçerek eklemek için iskele kurucu uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a3186-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="a3186-107">Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a3186-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="a3186-108">Örneğin, kayıt için kullanılan kodu oluşturmak için iskele kurucu istemeniz.</span><span class="sxs-lookup"><span data-stu-id="a3186-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="a3186-109">Oluşturulan kod kimlik RCL aynı kodu daha önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a3186-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="a3186-110">UI tam denetimini ve varsayılan RCL kullanmamak için bölümüne bakın [oluşturma tam kimlik UI kaynak](#full).</span><span class="sxs-lookup"><span data-stu-id="a3186-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="a3186-111">Yapmak uygulamaları **değil** dahil kimlik doğrulama RCL kimlik paketini eklemek için iskele kurucu uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a3186-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="a3186-112">Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="a3186-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="a3186-113">Gerekli kodu çoğunu iskele kurucu oluşturur ancak işlemi tamamlamak için projenize güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3186-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="a3186-114">Bu belge kimliği yapı iskelesi güncelleştirmesini tamamlamak için gereken adımları açıklar.</span><span class="sxs-lookup"><span data-stu-id="a3186-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="a3186-115">Kimlik iskele kurucu çalıştırdığınızda, bir *ScaffoldingReadme.txt* dosya proje dizininde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a3186-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="a3186-116">*ScaffoldingReadme.txt* dosyası ne kimlik yapı iskelesi güncelleştirmesini tamamlamak için gerekli olan genel yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="a3186-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="a3186-117">Bu belgede daha daha kapsamlı yönergeler içeren *ScaffoldingReadme.txt* dosya.</span><span class="sxs-lookup"><span data-stu-id="a3186-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="a3186-118">Dosya farklar gösterilmektedir ve dışında değişiklikleri geri olanak sağlayan bir kaynak denetim sistemi kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="a3186-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="a3186-119">Değişiklikleri kimlik iskele kurucu çalıştırdıktan sonra inceleyin.</span><span class="sxs-lookup"><span data-stu-id="a3186-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="a3186-120">Boş bir projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="a3186-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="a3186-121">Aşağıdaki vurgulanan çağrıları ekleme `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a3186-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="a3186-122">Varolan yetkilendirme olmadan Razor projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="a3186-122">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
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

<span data-ttu-id="a3186-123">Kimlik yapılandırılmıştır *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3186-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a3186-124">Daha fazla bilgi için bkz: [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a3186-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="a3186-125">Geçişler, UseAuthentication ve düzeni</span><span class="sxs-lookup"><span data-stu-id="a3186-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a3186-126">İçinde `Configure` yöntemi `Startup` sınıfı, arama [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sonra `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a3186-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="a3186-127">Düzen değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="a3186-127">Layout changes</span></span>

<span data-ttu-id="a3186-128">İsteğe bağlı: Kısmi oturum açma ekleme (`_LoginPartial`) Düzen dosyası için:</span><span class="sxs-lookup"><span data-stu-id="a3186-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="a3186-129">Yetkilendirme ile Razor projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="a3186-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="a3186-130">Bazı kimlik seçenekleri yapılandırılan *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3186-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a3186-131">Daha fazla bilgi için bkz: [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a3186-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="a3186-132">Varolan yetkilendirme olmadan bir MVC projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="a3186-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="a3186-133">İsteğe bağlı: Kısmi oturum açma ekleme (`_LoginPartial`) için *Views/Shared/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="a3186-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="a3186-134">Taşıma *Pages/Shared/_LoginPartial.cshtml* dosya *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a3186-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="a3186-135">Kimlik yapılandırılmıştır *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a3186-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="a3186-136">Daha fazla bilgi için IHostingStartup bakın.</span><span class="sxs-lookup"><span data-stu-id="a3186-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="a3186-137">Çağrı [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sonra `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="a3186-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="a3186-138">Yetkilendirme ile MVC projeye bir iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="a3186-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="a3186-139">Silme *sayfaları/paylaşılan* klasörüne ve o klasördeki dosyaları.</span><span class="sxs-lookup"><span data-stu-id="a3186-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="a3186-140">Tam kimlik UI kaynağı oluşturun</span><span class="sxs-lookup"><span data-stu-id="a3186-140">Create full identity UI source</span></span>

<span data-ttu-id="a3186-141">Kimlik UI tam denetimi korumak için kimlik iskele kurucu çalıştırın ve seçin **tüm dosyaları geçersiz kılma**.</span><span class="sxs-lookup"><span data-stu-id="a3186-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="a3186-142">Aşağıdaki vurgulanmış kodu varsayılan kimlik UI kimliği ile bir ASP.NET Core 2.1 web uygulamasında değiştirmek için değişiklikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="a3186-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="a3186-143">Bu kimlik UI tam denetime sahip olmasını yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a3186-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="a3186-144">Varsayılan kimlik aşağıdaki kodda değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="a3186-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="a3186-145">Aşağıdaki kod kümeleri [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), ve [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="a3186-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="a3186-146">Kayıt bir `IEmailSender` uygulamasında, örneğin:</span><span class="sxs-lookup"><span data-stu-id="a3186-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
