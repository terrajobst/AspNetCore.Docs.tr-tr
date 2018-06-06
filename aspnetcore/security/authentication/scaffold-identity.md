---
title: ASP.NET Core projelerinde iskele kimliği
author: rick-anderson
description: ASP.NET Core projesinde kimlik iskele öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 8933cf9c4063bd94f7f3a9ba53b5372b443eb7c8
ms.sourcegitcommit: d4cefc0c63550c64a8040b11867cc05efcfb7e86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34758758"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="1ff4c-103">ASP.NET Core projelerinde iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="1ff4c-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="1ff4c-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1ff4c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1ff4c-105">ASP.NET Core 2.1 ve üzeri sağlar [ASP.NET Core kimliği](xref:security/authentication/identity) olarak bir [Razor sınıf kitaplığı](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="1ff4c-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="1ff4c-106">Kimliği içeren uygulamaları kimlik Razor sınıf kitaplığı (RCL) bulunan kaynak koduna seçerek eklemek için iskele kurucu uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="1ff4c-107">Kodu değiştirin ve davranışını değiştirmek için kaynak kodu oluşturmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="1ff4c-108">Örneğin, kayıt için kullanılan kodu oluşturmak için iskele kurucu istemeniz.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="1ff4c-109">Oluşturulan kod kimlik RCL aynı kodu daha önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="1ff4c-110">Yapmak uygulamaları **değil** dahil kimlik doğrulama RCL kimlik paketini eklemek için iskele kurucu uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="1ff4c-111">Oluşturulacak kimlik kodu seçme seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="1ff4c-112">Gerekli kodu çoğunu iskele kurucu oluşturur ancak işlemi tamamlamak için projenize güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="1ff4c-113">Bu belge kimliği yapı iskelesi güncelleştirmesini tamamlamak için gereken adımları açıklar.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="1ff4c-114">Kimlik iskele kurucu çalıştırdığınızda, bir *ScaffoldingReadme.txt* dosya proje dizininde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="1ff4c-115">*ScaffoldingReadme.txt* dosyası ne kimlik yapı iskelesi güncelleştirmesini tamamlamak için gerekli olan genel yönergeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-115">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="1ff4c-116">Bu belgeyi okuma değerinden daha kapsamlı yönergeler içeren *ScaffoldingReadme.txt* dosya.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="1ff4c-117">Dosya farklar gösterilmektedir ve dışında değişiklikleri geri olanak sağlayan bir kaynak denetim sistemi kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="1ff4c-118">Değişiklikleri kimlik iskele kurucu çalıştırdıktan sonra inceleyin.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="1ff4c-119">Boş bir projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="1ff4c-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="1ff4c-120">Aşağıdaki vurgulanan çağrıları ekleme `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="1ff4c-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="1ff4c-121">Varolan yetkilendirme olmadan Razor projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="1ff4c-121">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="1ff4c-122">Kimlik yapılandırılmıştır *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="1ff4c-123">Daha fazla bilgi için bkz: [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="1ff4c-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="1ff4c-124">Geçişler, UseAuthentication ve düzeni</span><span class="sxs-lookup"><span data-stu-id="1ff4c-124">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="1ff4c-125">İçinde `Configure` yöntemi `Startup` sınıfı, arama [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sonra `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="1ff4c-125">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="1ff4c-126">Düzen değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="1ff4c-126">Layout changes</span></span>

<span data-ttu-id="1ff4c-127">İsteğe bağlı: Kısmi oturum açma ekleme (`_LoginPartial`) Düzen dosyası için:</span><span class="sxs-lookup"><span data-stu-id="1ff4c-127">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="1ff4c-128">Yetkilendirme ile Razor projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="1ff4c-128">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="1ff4c-129">Bazı kimlik seçenekleri yapılandırılan *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-129">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="1ff4c-130">Daha fazla bilgi için bkz: [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="1ff4c-130">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="1ff4c-131">Varolan yetkilendirme olmadan bir MVC projeye iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="1ff4c-131">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="1ff4c-132">İsteğe bağlı: Kısmi oturum açma ekleme (`_LoginPartial`) için *Views/Shared/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="1ff4c-132">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="1ff4c-133">Taşıma *Pages/Shared/_LoginPartial.cshtml* dosya *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1ff4c-133">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="1ff4c-134">Kimlik yapılandırılmıştır *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-134">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="1ff4c-135">Daha fazla bilgi için IHostingStartup bakın.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-135">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="1ff4c-136">Çağrı [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) sonra `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="1ff4c-136">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="1ff4c-137">Yetkilendirme ile MVC projeye bir iskele kimliği</span><span class="sxs-lookup"><span data-stu-id="1ff4c-137">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="1ff4c-138">Silme *sayfaları/paylaşılan* klasörüne ve o klasördeki dosyaları.</span><span class="sxs-lookup"><span data-stu-id="1ff4c-138">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
