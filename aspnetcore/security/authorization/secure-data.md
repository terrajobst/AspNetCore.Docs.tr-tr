---
title: Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı veri yetkilendirme tarafından korunan bir Razor sayfalarının uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 01/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 32ced054ba559e66de4a300137d56b7c81a4149b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276386"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="6dcb1-104">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6dcb1-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="6dcb1-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="6dcb1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="6dcb1-106">Bu öğretici bir ASP.NET Core web uygulamasının yetkilendirme tarafından korunan kullanıcı verileri ile nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="6dcb1-107">Kimliği doğrulanmış (kayıtlı) kullanıcılar Kişiler listesini görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="6dcb1-108">Üç güvenlik grupları vardır:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-108">There are three security groups:</span></span>

* <span data-ttu-id="6dcb1-109">**Kayıtlı kullanıcıların** tüm onaylanmış verilerini görüntüleyebilir ve düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="6dcb1-110">**Yöneticileri** onaylayabilir veya kişi verilerini reddedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="6dcb1-111">Yalnızca onaylanan kişilere kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="6dcb1-112">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="6dcb1-113">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) imzalanır.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="6dcb1-114">Rick yalnızca onaylanan kişilere görüntülemek ve **Düzenle**/**silmek**/**Yeni Oluştur** kendi kişiler için bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="6dcb1-115">Yalnızca son kaydı görüntüler Rick tarafından oluşturulan **Düzenle** ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="6dcb1-116">Bir yöneticinin "Onaylandı" durumuna kadar diğer kullanıcılar son kaydı görmez.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Önceki görüntü açıklanan](secure-data/_static/rick.png)

<span data-ttu-id="6dcb1-118">Aşağıdaki görüntüde `manager@contoso.com` ve yöneticileri rolünde imzalı:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/manager1.png)

<span data-ttu-id="6dcb1-120">Aşağıdaki resimde yöneticileri Ayrıntılar görünümü bir iletişim gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-120">The following image shows the managers details view of a contact:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/manager.png)

<span data-ttu-id="6dcb1-122">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="6dcb1-123">Aşağıdaki görüntüde `admin@contoso.com` ve yöneticiler rolünde imzalı:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/admin.png)

<span data-ttu-id="6dcb1-125">Yönetici, tüm ayrıcalıklarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-125">The administrator has all privileges.</span></span> <span data-ttu-id="6dcb1-126">Depoladığından, herhangi bir kişiyi okuma/düzenleme/silme yapabilir ve kişiler durumunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="6dcb1-127">Uygulama tarafından oluşturulan [iskele](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="6dcb1-128">Örnek aşağıdaki yetkilendirme işleyicileri içerir:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="6dcb1-129">`ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi veri düzenleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="6dcb1-130">`ContactManagerAuthorizationHandler`: Onaylamak veya reddetmek kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="6dcb1-131">`ContactAdministratorsAuthorizationHandler`: Yöneticilerinin onaylama veya reddetme kişiler ve kişi düzenleme/silme sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6dcb1-132">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6dcb1-132">Prerequisites</span></span>

<span data-ttu-id="6dcb1-133">Bu öğretici Gelişmiş.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-133">This tutorial is advanced.</span></span> <span data-ttu-id="6dcb1-134">Hakkında bilginiz olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-134">You should be familiar with:</span></span>

* [<span data-ttu-id="6dcb1-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6dcb1-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6dcb1-136">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="6dcb1-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="6dcb1-137">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="6dcb1-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6dcb1-138">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="6dcb1-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6dcb1-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="6dcb1-140">Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürüm için.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="6dcb1-141">Bu öğretici ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="6dcb1-142">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="6dcb1-143">Tamamlanan uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="6dcb1-143">The starter and completed app</span></span>

<span data-ttu-id="6dcb1-144">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="6dcb1-145">[Test](#test-the-completed-app) tamamlanan uygulama ile güvenlik özellikleri hakkında bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="6dcb1-146">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="6dcb1-146">The starter app</span></span>

<span data-ttu-id="6dcb1-147">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="6dcb1-148">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="6dcb1-149">Kullanıcı verileri güvenli</span><span class="sxs-lookup"><span data-stu-id="6dcb1-149">Secure user data</span></span>

<span data-ttu-id="6dcb1-150">Aşağıdaki bölümlerde güvenli kullanıcı veri uygulaması oluşturmak için tüm önemli adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="6dcb1-151">Tamamlanan projesine başvurması daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="6dcb1-152">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="6dcb1-152">Tie the contact data to the user</span></span>

<span data-ttu-id="6dcb1-153">ASP.NET kullanan [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerini, ancak diğer kullanıcılar verileri düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="6dcb1-154">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="6dcb1-155">`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="6dcb1-156">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="6dcb1-157">Yeni geçiş oluştur ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="6dcb1-158">HTTPS ve kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="6dcb1-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="6dcb1-159">Ekleme [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) için `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="6dcb1-160">İçinde `ConfigureServices` yöntemi *haline* dosya, ekleme [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) yetkilendirme Filtresi:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="6dcb1-161">Visual Studio kullanıyorsanız, HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="6dcb1-162">HTTPS için HTTP isteklerini yeniden yönlendirmek için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="6dcb1-163">Visual Studio Code kullanarak ya da yerel bir platformda sınama, HTTPS için bir test sertifikası içermez:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="6dcb1-164">Ayarlama `"LocalTest:skipHTTPS": true` içinde *appsettings. Developement.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-164">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="6dcb1-165">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="6dcb1-165">Require authenticated users</span></span>

<span data-ttu-id="6dcb1-166">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="6dcb1-167">Kimlik doğrulaması ile Razor sayfasını, denetleyici veya eylem yöntemi düzeyinde dışında seçebilirsiniz `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="6dcb1-168">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfalarının ve denetleyiciler korur.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="6dcb1-169">Kimlik doğrulaması varsayılan olarak gerekli yeni denetleyicileri ve dahil etmek için Razor sayfalarının bağlı olan daha güvenli olan `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="6dcb1-170">Kimliği doğrulanmış, tüm kullanıcılar gereksinimiyle [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) ve [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) çağrıları gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="6dcb1-171">Güncelleştirme `ConfigureServices` aşağıdaki değişiklikleri:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="6dcb1-172">Out açıklama `AuthorizeFolder` ve `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="6dcb1-173">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="6dcb1-174">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine, bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri sınıflandırıp hakkında ve iletişim sayfaları.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="6dcb1-175">Ekleme `[AllowAnonymous]` için [LoginModel ve RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="6dcb1-176">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="6dcb1-176">Configure the test account</span></span>

<span data-ttu-id="6dcb1-177">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="6dcb1-178">Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="6dcb1-179">Proje dizinden parola ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="6dcb1-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="6dcb1-180">Güçlü bir parola kullanmıyorsanız, ne zaman özel durum `SeedData.Initialize` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-180">If you don't use a strong password, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="6dcb1-181">Güncelleştirme `Main` test parola kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-181">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="6dcb1-182">Test hesapları oluşturabilir ve kişileri güncelleştir</span><span class="sxs-lookup"><span data-stu-id="6dcb1-182">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="6dcb1-183">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturmak için sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-183">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="6dcb1-184">Yönetici kullanıcı kimliği ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-184">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="6dcb1-185">"Gönderildi" ve bir "Reddedilen" kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-185">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="6dcb1-186">Kullanıcı kimliği ve durum tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-186">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="6dcb1-187">Yalnızca bir kişinin gösterilir:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-187">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="6dcb1-188">Sahibi, Yöneticisi ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6dcb1-188">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="6dcb1-189">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-189">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6dcb1-190">`ContactIsOwnerAuthorizationHandler` Bir kaynakta hareket kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-190">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="6dcb1-191">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) geçerli kimliği doğrulanmış kullanıcı kişi sahibi ise.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-191">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="6dcb1-192">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-192">Authorization handlers generally:</span></span>

* <span data-ttu-id="6dcb1-193">Dönüş `context.Succeed` zaman gereksinimleri karşılandıktan.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-193">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="6dcb1-194">Dönüş `Task.CompletedTask` zaman olmayan gereksinimlerin karşılanması.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-194">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="6dcb1-195">`Task.CompletedTask` ne başarılı veya başarısız olduğunu&mdash;çalıştırmak diğer yetkilendirme işleyicileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-195">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="6dcb1-196">Açıkça başarısız gerekiyorsa, dönüş [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-196">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="6dcb1-197">Uygulamanın kendi veri düzenleme/silme/oluşturmak için kişi sahipleri verir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-197">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="6dcb1-198">`ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetlemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-198">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="6dcb1-199">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="6dcb1-199">Create a manager authorization handler</span></span>

<span data-ttu-id="6dcb1-200">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-200">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6dcb1-201">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-201">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="6dcb1-202">Yalnızca Yöneticiler onaylayabilir veya reddedebilirsiniz içerik değişikliklerini (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-202">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="6dcb1-203">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="6dcb1-203">Create an administrator authorization handler</span></span>

<span data-ttu-id="6dcb1-204">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-204">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6dcb1-205">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-205">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="6dcb1-206">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-206">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="6dcb1-207">Yetkilendirme işleyicileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-207">Register the authorization handlers</span></span>

<span data-ttu-id="6dcb1-208">Entity Framework Çekirdek kullanarak Hizmetleri kayıtlı, için [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-208">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="6dcb1-209">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-209">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="6dcb1-210">Kullanılabilir hizmet koleksiyonuyla işleyicileri kaydolmayı `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-210">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6dcb1-211">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-211">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="6dcb1-212">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-212">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="6dcb1-213">Olup olmadıklarını teklileri EF kullanmayın ve gerekli tüm bilgileri de olduğundan `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-213">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="6dcb1-214">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-214">Support authorization</span></span>

<span data-ttu-id="6dcb1-215">Bu bölümde, Razor sayfalarının güncelleştirin ve işlemleri gereksinimleri sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-215">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="6dcb1-216">Gözden geçirme kişi operations gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="6dcb1-216">Review the contact operations requirements class</span></span>

<span data-ttu-id="6dcb1-217">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-217">Review the `ContactOperations` class.</span></span> <span data-ttu-id="6dcb1-218">Bu sınıf gereksinimlerini içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-218">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="6dcb1-219">Razor sayfalar için temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="6dcb1-219">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="6dcb1-220">Kişiler Razor sayfalarının kullanılan hizmetleri içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-220">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="6dcb1-221">Taban sınıfı başlatma kodun bir konuma yerleştirir:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-221">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="6dcb1-222">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-222">The preceding code:</span></span>

* <span data-ttu-id="6dcb1-223">Ekler `IAuthorizationService` yetkilendirme işleyicilerine erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-223">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="6dcb1-224">Kimlik ekler `UserManager` hizmet.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-224">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="6dcb1-225">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-225">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="6dcb1-226">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="6dcb1-226">Update the CreateModel</span></span>

<span data-ttu-id="6dcb1-227">Kullanılacak Oluştur sayfası modeli Oluşturucusu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-227">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="6dcb1-228">Güncelleştirme `CreateModel.OnPostAsync` yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-228">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="6dcb1-229">Kullanıcı kimliği eklemek `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-229">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="6dcb1-230">Kullanıcının kişiler oluşturmak için izne sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-230">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="6dcb1-231">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="6dcb1-231">Update the IndexModel</span></span>

<span data-ttu-id="6dcb1-232">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilen şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-232">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="6dcb1-233">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="6dcb1-233">Update the EditModel</span></span>

<span data-ttu-id="6dcb1-234">Kullanıcının kişinin sahip olduğu doğrulamak için bir yetkilendirme işleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-234">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="6dcb1-235">Kaynak Yetkilendirme doğrulandığı için `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-235">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="6dcb1-236">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-236">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="6dcb1-237">Kaynak tabanlı bir yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-237">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="6dcb1-238">Uygulama sayfası modelinde yükleme ya da işleyici içinde yükleme kaynağına erişimi olduğunda denetimlerinin gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-238">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="6dcb1-239">Sık sık kaynak anahtarı geçirerek kaynak erişir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-239">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="6dcb1-240">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="6dcb1-240">Update the DeleteModel</span></span>

<span data-ttu-id="6dcb1-241">Kullanıcı, kişi hakkında silme iznine sahip doğrulamak için yetkilendirme işleyicisi kullanmak için silme sayfa modeli güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-241">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="6dcb1-242">Yetkilendirme hizmeti görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-242">Inject the authorization service into the views</span></span>

<span data-ttu-id="6dcb1-243">Şu anda, UI gösterir düzenleyebilir ve kullanıcı değiştirilemiyor veri bağlantıları silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-243">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="6dcb1-244">Kullanıcı arabirimini görünümlerine yetkilendirme işleyici uygulayarak düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-244">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="6dcb1-245">Yetkilendirme hizmetinde Ekle *Views/_ViewImports.cshtml* tüm görünümler kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-245">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="6dcb1-246">Önceki biçimlendirme birkaç ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-246">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="6dcb1-247">Güncelleştirme **Düzenle** ve **silmek** içinde bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar için işlenen için:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-247">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="6dcb1-248">Veri değiştirme iznine sahip olmayan kullanıcılardan gelen bağlantılar gizleme uygulama güvenli değil.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-248">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="6dcb1-249">Bağlantılar gizleme uygulama daha kullanıcı dostu yalnızca geçerli bağlantılar görüntüleyerek yapmaz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-249">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="6dcb1-250">Kullanıcıları düzenleme çağırma ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'lerini korsan saldırılarına.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-250">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="6dcb1-251">Razor sayfasını veya denetleyicisi verileri korumak için erişim denetimleri zorunlu gerekir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-251">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="6dcb1-252">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="6dcb1-252">Update Details</span></span>

<span data-ttu-id="6dcb1-253">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-253">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="6dcb1-254">Ayrıntılar sayfası modeli güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-254">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="6dcb1-255">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-255">Test the completed app</span></span>

<span data-ttu-id="6dcb1-256">Visual Studio Code kullanarak ya da yerel bir platformda sınama, HTTPS için bir test sertifikası içermez:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-256">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="6dcb1-257">Ayarlama `"LocalTest:skipHTTPS": true` içinde *appsettings. Developement.JSON* HTTPS gereksinim atlamak için dosya.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-257">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="6dcb1-258">Bir geliştirme makinede yalnızca HTTPS atla.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-258">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="6dcb1-259">Uygulamanın kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-259">If the app has contacts:</span></span>

* <span data-ttu-id="6dcb1-260">Tüm kayıtları silin `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-260">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="6dcb1-261">Veritabanını oluşturmak için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-261">Restart the app to seed the database.</span></span>

<span data-ttu-id="6dcb1-262">Bir kullanıcı kişiler göz atmak için kaydolun.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-262">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="6dcb1-263">Üç farklı tarayıcılar (veya ıncognito/InPrivate sürümler) başlatmak için tamamlanan uygulama test etmek için kolay bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-263">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="6dcb1-264">Bir tarayıcıda, yeni bir kullanıcı kaydı (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-264">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="6dcb1-265">Her tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-265">Sign in to each browser with a different user.</span></span> <span data-ttu-id="6dcb1-266">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-266">Verify the following operations:</span></span>

* <span data-ttu-id="6dcb1-267">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-267">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="6dcb1-268">Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-268">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="6dcb1-269">Yöneticileri onaylayın veya kişi verilerini reddedin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-269">Managers can approve or reject contact data.</span></span> <span data-ttu-id="6dcb1-270">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeler.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-270">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="6dcb1-271">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-271">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="6dcb1-272">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="6dcb1-272">User</span></span>| <span data-ttu-id="6dcb1-273">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="6dcb1-273">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="6dcb1-274">Düzenle/kendi veri silmek</span><span class="sxs-lookup"><span data-stu-id="6dcb1-274">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="6dcb1-275">Onayla/Reddet ve düzenleme/silme veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="6dcb1-275">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="6dcb1-276">Düzenleme/silme ve tüm veri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="6dcb1-276">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="6dcb1-277">Bir kişi yöneticinin tarayıcıda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="6dcb1-278">Delete URL'sini kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="6dcb1-279">Bu bağlantıları test kullanıcısı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcıya yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="6dcb1-280">Başlangıç uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6dcb1-280">Create the starter app</span></span>

* <span data-ttu-id="6dcb1-281">"ContactManager" adlı bir Razor sayfalarının uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6dcb1-281">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="6dcb1-282">Uygulama oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="6dcb1-283">Aşağıdaki örnekte kullanılan ad ad alanınızı eşleşecek şekilde "ContactManager" adı.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-283">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * <span data-ttu-id="6dcb1-285">`-uld` SQLite yerine LocalDB belirtir</span><span class="sxs-lookup"><span data-stu-id="6dcb1-285">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="6dcb1-286">Aşağıdakileri ekleyin `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-286">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="6dcb1-287">İskele `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-287">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="6dcb1-288">Güncelleştirme **ContactManager** içinde yer alan bağlayıcı *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-288">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="6dcb1-289">İlk geçişin iskelesi ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-289">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="6dcb1-290">Oluşturma, düzenleme ve bir kişi silme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="6dcb1-291">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="6dcb1-291">Seed the database</span></span>

<span data-ttu-id="6dcb1-292">Ekleme `SeedData` sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-292">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="6dcb1-293">Örnek indirdiğiniz, kopyalayabilirsiniz *SeedData.cs* dosya *veri* başlangıç projesinin klasör.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-293">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="6dcb1-294">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="6dcb1-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="6dcb1-295">Uygulama veritabanı sağlanmış sınayın.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-295">Test that the app seeded the database.</span></span> <span data-ttu-id="6dcb1-296">Seed yöntemi kişi DB herhangi bir satır varsa, çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="6dcb1-297">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6dcb1-297">Additional resources</span></span>

* <span data-ttu-id="6dcb1-298">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="6dcb1-298">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="6dcb1-299">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="6dcb1-299">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="6dcb1-300">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="6dcb1-300">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="6dcb1-301">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6dcb1-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
