---
title: "Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma"
author: rick-anderson
description: "Kullanıcı veri yetkilendirme tarafından korunan bir Razor sayfalarının uygulaması oluşturmayı öğrenin. SSL, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="19495-104">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="19495-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="19495-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="19495-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="19495-106">Bu öğretici bir ASP.NET Core web uygulamasının yetkilendirme tarafından korunan kullanıcı verileri ile nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19495-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="19495-107">Kimliği doğrulanmış (kayıtlı) kullanıcılar Kişiler listesini görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="19495-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="19495-108">Üç güvenlik grupları vardır:</span><span class="sxs-lookup"><span data-stu-id="19495-108">There are three security groups:</span></span>

* <span data-ttu-id="19495-109">**Kayıtlı kullanıcıların** tüm onaylanmış verilerini görüntüleyebilir ve düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="19495-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="19495-110">**Yöneticileri** onaylayabilir veya kişi verilerini reddedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19495-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="19495-111">Yalnızca onaylanan kişilere kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="19495-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="19495-112">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19495-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="19495-113">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) imzalanır.</span><span class="sxs-lookup"><span data-stu-id="19495-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="19495-114">Rick yalnızca onaylanan kişilere görüntülemek ve **Düzenle**/**silmek**/**Yeni Oluştur** kendi kişiler için bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="19495-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="19495-115">Yalnızca son kaydı görüntüler Rick tarafından oluşturulan **Düzenle** ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="19495-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="19495-116">Bir yöneticinin "Onaylandı" durumuna kadar diğer kullanıcılar son kaydı görmez.</span><span class="sxs-lookup"><span data-stu-id="19495-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Önceki görüntü açıklanan](secure-data/_static/rick.png)

<span data-ttu-id="19495-118">Aşağıdaki görüntüde `manager@contoso.com` ve yöneticileri rolünde imzalı:</span><span class="sxs-lookup"><span data-stu-id="19495-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/manager1.png)

<span data-ttu-id="19495-120">Aşağıdaki resimde yöneticileri Ayrıntılar görünümü bir iletişim gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="19495-120">The following image shows the managers details view of a contact:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/manager.png)

<span data-ttu-id="19495-122">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="19495-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="19495-123">Aşağıdaki görüntüde `admin@contoso.com` ve yöneticiler rolünde imzalı:</span><span class="sxs-lookup"><span data-stu-id="19495-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/admin.png)

<span data-ttu-id="19495-125">Yönetici, tüm ayrıcalıklarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="19495-125">The administrator has all privileges.</span></span> <span data-ttu-id="19495-126">Depoladığından, herhangi bir kişiyi okuma/düzenleme/silme yapabilir ve kişiler durumunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19495-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="19495-127">Uygulama tarafından oluşturulan [iskele](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="19495-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="19495-128">Örnek aşağıdaki yetkilendirme işleyicileri içerir:</span><span class="sxs-lookup"><span data-stu-id="19495-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="19495-129">`ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi veri düzenleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="19495-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="19495-130">`ContactManagerAuthorizationHandler`: Onaylamak veya reddetmek kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="19495-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="19495-131">`ContactAdministratorsAuthorizationHandler`: Yöneticilerinin onaylama veya reddetme kişiler ve kişi düzenleme/silme sağlar.</span><span class="sxs-lookup"><span data-stu-id="19495-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19495-132">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="19495-132">Prerequisites</span></span>

<span data-ttu-id="19495-133">Bu öğretici Gelişmiş.</span><span class="sxs-lookup"><span data-stu-id="19495-133">This tutorial is advanced.</span></span> <span data-ttu-id="19495-134">Hakkında bilginiz olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="19495-134">You should be familiar with:</span></span>

* [<span data-ttu-id="19495-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19495-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="19495-136">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="19495-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="19495-137">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="19495-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="19495-138">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="19495-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="19495-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="19495-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="19495-140">Bkz: [bu PDF dosyası](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürüm için.</span><span class="sxs-lookup"><span data-stu-id="19495-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="19495-141">Bu öğretici ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="19495-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="19495-142">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="19495-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="19495-143">Tamamlanan uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="19495-143">The starter and completed app</span></span>

<span data-ttu-id="19495-144">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="19495-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="19495-145">[Test](#test-the-completed-app) tamamlanan uygulama ile güvenlik özellikleri hakkında bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="19495-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="19495-146">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="19495-146">The starter app</span></span>

<span data-ttu-id="19495-147">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="19495-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="19495-148">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="19495-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="19495-149">Kullanıcı verileri güvenli</span><span class="sxs-lookup"><span data-stu-id="19495-149">Secure user data</span></span>

<span data-ttu-id="19495-150">Aşağıdaki bölümlerde güvenli kullanıcı veri uygulaması oluşturmak için tüm önemli adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="19495-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="19495-151">Tamamlanan projesine başvurması daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="19495-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="19495-152">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="19495-152">Tie the contact data to the user</span></span>

<span data-ttu-id="19495-153">ASP.NET kullanan [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerini, ancak diğer kullanıcılar verileri düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19495-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="19495-154">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="19495-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="19495-155">`OwnerID`kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="19495-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="19495-156">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="19495-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="19495-157">Yeni geçiş oluştur ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="19495-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="19495-158">SSL ve kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="19495-158">Require SSL and authenticated users</span></span>

<span data-ttu-id="19495-159">Ekleme [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) için `Startup`:</span><span class="sxs-lookup"><span data-stu-id="19495-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="19495-160">İçinde `ConfigureServices` yöntemi *haline* dosya, ekleme [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) yetkilendirme Filtresi:</span><span class="sxs-lookup"><span data-stu-id="19495-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

<span data-ttu-id="19495-161">Visual Studio kullanıyorsanız, SSL'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="19495-161">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="19495-162">HTTPS için HTTP isteklerini yeniden yönlendirmek için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="19495-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="19495-163">Visual Studio Code kullanarak ya da yerel bir platformda sınama, SSL için bir test sertifikası içermez:</span><span class="sxs-lookup"><span data-stu-id="19495-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="19495-164">Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings. Developement.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="19495-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="19495-165">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="19495-165">Require authenticated users</span></span>

<span data-ttu-id="19495-166">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="19495-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="19495-167">Kimlik doğrulaması ile Razor sayfasını, denetleyici veya eylem yöntemi düzeyinde dışında seçebilirsiniz `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="19495-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="19495-168">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfalarının ve denetleyiciler korur.</span><span class="sxs-lookup"><span data-stu-id="19495-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="19495-169">Kimlik doğrulaması varsayılan olarak gerekli yeni denetleyicileri ve dahil etmek için Razor sayfalarının bağlı olan daha güvenli olan `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="19495-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="19495-170">Aşağıdakileri ekleyin `ConfigureServices` yöntemi *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="19495-170">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

<span data-ttu-id="19495-171">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine, bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri sınıflandırıp hakkında ve iletişim sayfaları.</span><span class="sxs-lookup"><span data-stu-id="19495-171">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="19495-172">Ekleme `[AllowAnonymous]` için [LoginModel ve RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="19495-172">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="19495-173">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="19495-173">Configure the test account</span></span>

<span data-ttu-id="19495-174">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="19495-174">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="19495-175">Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19495-175">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="19495-176">Proje dizinden parola ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="19495-176">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="19495-177">Güncelleştirme `Main` test parola kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="19495-177">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="19495-178">Test hesapları oluşturabilir ve kişileri güncelleştir</span><span class="sxs-lookup"><span data-stu-id="19495-178">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="19495-179">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturmak için sınıfı:</span><span class="sxs-lookup"><span data-stu-id="19495-179">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="19495-180">Yönetici kullanıcı kimliği ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="19495-180">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="19495-181">"Gönderildi" ve bir "Reddedilen" kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="19495-181">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="19495-182">Kullanıcı kimliği ve durum tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19495-182">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="19495-183">Yalnızca bir kişinin gösterilir:</span><span class="sxs-lookup"><span data-stu-id="19495-183">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="19495-184">Sahibi, Yöneticisi ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="19495-184">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="19495-185">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="19495-185">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="19495-186">`ContactIsOwnerAuthorizationHandler` Bir kaynakta hareket kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="19495-186">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="19495-187">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) geçerli kimliği doğrulanmış kullanıcı kişi sahibi ise.</span><span class="sxs-lookup"><span data-stu-id="19495-187">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="19495-188">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="19495-188">Authorization handlers generally:</span></span>

* <span data-ttu-id="19495-189">Dönüş `context.Succeed` zaman gereksinimleri karşılandıktan.</span><span class="sxs-lookup"><span data-stu-id="19495-189">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="19495-190">Dönüş `Task.CompletedTask` zaman olmayan gereksinimlerin karşılanması.</span><span class="sxs-lookup"><span data-stu-id="19495-190">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="19495-191">`Task.CompletedTask`ne başarılı veya başarısız olduğunu&mdash;çalıştırmak diğer yetkilendirme işleyicileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="19495-191">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="19495-192">Açıkça başarısız gerekiyorsa, dönüş [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="19495-192">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="19495-193">Uygulamanın kendi veri düzenleme/silme/oluşturmak için kişi sahipleri verir.</span><span class="sxs-lookup"><span data-stu-id="19495-193">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="19495-194">`ContactIsOwnerAuthorizationHandler`gereksinim parametrede aktarılan işlemi denetlemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="19495-194">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="19495-195">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="19495-195">Create a manager authorization handler</span></span>

<span data-ttu-id="19495-196">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="19495-196">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="19495-197">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="19495-197">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="19495-198">Yalnızca Yöneticiler onaylayabilir veya reddedebilirsiniz içerik değişikliklerini (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="19495-198">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="19495-199">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="19495-199">Create an administrator authorization handler</span></span>

<span data-ttu-id="19495-200">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="19495-200">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="19495-201">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="19495-201">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="19495-202">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="19495-202">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="19495-203">Yetkilendirme işleyicileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="19495-203">Register the authorization handlers</span></span>

<span data-ttu-id="19495-204">Entity Framework Çekirdek kullanarak Hizmetleri kayıtlı, için [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="19495-204">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="19495-205">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="19495-205">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="19495-206">Kullanılabilir hizmet koleksiyonuyla işleyicileri kaydolmayı `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="19495-206">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="19495-207">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="19495-207">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="19495-208">`ContactAdministratorsAuthorizationHandler`ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="19495-208">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="19495-209">Olup olmadıklarını teklileri EF kullanmayın ve gerekli tüm bilgileri de olduğundan `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19495-209">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="19495-210">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="19495-210">Support authorization</span></span>

<span data-ttu-id="19495-211">Bu bölümde, Razor sayfalarının güncelleştirin ve işlemleri gereksinimleri sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19495-211">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="19495-212">Gözden geçirme kişi operations gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="19495-212">Review the contact operations requirements class</span></span>

<span data-ttu-id="19495-213">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="19495-213">Review the `ContactOperations` class.</span></span> <span data-ttu-id="19495-214">Bu sınıf gereksinimlerini içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="19495-214">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="19495-215">Razor sayfalar için temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="19495-215">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="19495-216">Kişiler Razor sayfalarının kullanılan hizmetleri içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="19495-216">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="19495-217">Taban sınıfı başlatma kodun bir konuma yerleştirir:</span><span class="sxs-lookup"><span data-stu-id="19495-217">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="19495-218">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="19495-218">The preceding code:</span></span>

* <span data-ttu-id="19495-219">Ekler `IAuthorizationService` yetkilendirme işleyicilerine erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="19495-219">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="19495-220">Kimlik ekler `UserManager` hizmet.</span><span class="sxs-lookup"><span data-stu-id="19495-220">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="19495-221">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="19495-221">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="19495-222">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="19495-222">Update the CreateModel</span></span>

<span data-ttu-id="19495-223">Kullanılacak Oluştur sayfası modeli Oluşturucusu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="19495-223">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="19495-224">Güncelleştirme `CreateModel.OnPostAsync` yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="19495-224">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="19495-225">Kullanıcı kimliği eklemek `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="19495-225">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="19495-226">Kullanıcının kişiler oluşturmak için izne sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="19495-226">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="19495-227">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="19495-227">Update the IndexModel</span></span>

<span data-ttu-id="19495-228">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilen şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="19495-228">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="19495-229">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="19495-229">Update the EditModel</span></span>

<span data-ttu-id="19495-230">Kullanıcının kişinin sahip olduğu doğrulamak için bir yetkilendirme işleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19495-230">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="19495-231">Kaynak Yetkilendirme doğrulandığı için `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="19495-231">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="19495-232">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="19495-232">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="19495-233">Kaynak tabanlı bir yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="19495-233">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="19495-234">Uygulama sayfası modelinde yükleme ya da işleyici içinde yükleme kaynağına erişimi olduğunda denetimlerinin gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="19495-234">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="19495-235">Sık sık kaynak anahtarı geçirerek kaynak erişir.</span><span class="sxs-lookup"><span data-stu-id="19495-235">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="19495-236">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="19495-236">Update the DeleteModel</span></span>

<span data-ttu-id="19495-237">Kullanıcı, kişi hakkında silme iznine sahip doğrulamak için yetkilendirme işleyicisi kullanmak için silme sayfa modeli güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="19495-237">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="19495-238">Yetkilendirme hizmeti görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="19495-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="19495-239">Şu anda, UI gösterir düzenleyebilir ve kullanıcı değiştirilemiyor veri bağlantıları silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19495-239">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="19495-240">Kullanıcı arabirimini görünümlerine yetkilendirme işleyici uygulayarak düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="19495-240">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="19495-241">Yetkilendirme hizmetinde Ekle *Views/_ViewImports.cshtml* tüm görünümler kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="19495-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="19495-242">Önceki biçimlendirme birkaç ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="19495-242">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="19495-243">Güncelleştirme **Düzenle** ve **silmek** içinde bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar için işlenen için:</span><span class="sxs-lookup"><span data-stu-id="19495-243">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="19495-244">Veri değiştirme iznine sahip olmayan kullanıcılardan gelen bağlantılar gizleme uygulama güvenli değil.</span><span class="sxs-lookup"><span data-stu-id="19495-244">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="19495-245">Bağlantılar gizleme uygulama daha kullanıcı dostu yalnızca geçerli bağlantılar görüntüleyerek yapmaz.</span><span class="sxs-lookup"><span data-stu-id="19495-245">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="19495-246">Kullanıcıları düzenleme çağırma ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'lerini korsan saldırılarına.</span><span class="sxs-lookup"><span data-stu-id="19495-246">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="19495-247">Razor sayfasını veya denetleyicisi verileri korumak için erişim denetimleri zorunlu gerekir.</span><span class="sxs-lookup"><span data-stu-id="19495-247">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="19495-248">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="19495-248">Update Details</span></span>

<span data-ttu-id="19495-249">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="19495-249">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="19495-250">Ayrıntılar sayfası modeli güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="19495-250">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="19495-251">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="19495-251">Test the completed app</span></span>

<span data-ttu-id="19495-252">Visual Studio Code kullanarak ya da yerel bir platformda sınama, SSL için bir test sertifikası içermez:</span><span class="sxs-lookup"><span data-stu-id="19495-252">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="19495-253">Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings. Developement.JSON* SSL gereksinimlerini atlamak için dosya.</span><span class="sxs-lookup"><span data-stu-id="19495-253">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="19495-254">SSL yalnızca geliştirme makinenizde atlayın.</span><span class="sxs-lookup"><span data-stu-id="19495-254">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="19495-255">Uygulamanın kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="19495-255">If the app has contacts:</span></span>

* <span data-ttu-id="19495-256">Tüm kayıtları silin `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="19495-256">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="19495-257">Veritabanını oluşturmak için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="19495-257">Restart the app to seed the database.</span></span>

<span data-ttu-id="19495-258">Bir kullanıcı kişiler göz atmak için kaydolun.</span><span class="sxs-lookup"><span data-stu-id="19495-258">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="19495-259">Üç farklı tarayıcılar (veya ıncognito/InPrivate sürümler) başlatmak için tamamlanan uygulama test etmek için kolay bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="19495-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="19495-260">Bir tarayıcıda, yeni bir kullanıcı kaydı (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="19495-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="19495-261">Her tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="19495-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="19495-262">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="19495-262">Verify the following operations:</span></span>

* <span data-ttu-id="19495-263">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="19495-263">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="19495-264">Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="19495-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="19495-265">Yöneticileri onaylayın veya kişi verilerini reddedin.</span><span class="sxs-lookup"><span data-stu-id="19495-265">Managers can approve or reject contact data.</span></span> <span data-ttu-id="19495-266">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeler.</span><span class="sxs-lookup"><span data-stu-id="19495-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="19495-267">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="19495-267">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="19495-268">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="19495-268">User</span></span>| <span data-ttu-id="19495-269">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="19495-269">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="19495-270">Düzenle/kendi veri silmek</span><span class="sxs-lookup"><span data-stu-id="19495-270">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="19495-271">Onayla/Reddet ve düzenleme/silme veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="19495-271">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="19495-272">Düzenleme/silme ve tüm veri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="19495-272">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="19495-273">Bir kişi yöneticinin tarayıcıda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="19495-273">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="19495-274">Delete URL'sini kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="19495-274">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="19495-275">Bu bağlantıları test kullanıcısı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcıya yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="19495-275">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="19495-276">Başlangıç uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="19495-276">Create the starter app</span></span>

* <span data-ttu-id="19495-277">"ContactManager" adlı bir Razor sayfalarının uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="19495-277">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="19495-278">Uygulama oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="19495-278">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="19495-279">Aşağıdaki örnekte kullanılan ad ad alanınızı eşleşecek şekilde "ContactManager" adı.</span><span class="sxs-lookup"><span data-stu-id="19495-279">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="19495-280">`-uld`SQLite yerine LocalDB belirtir</span><span class="sxs-lookup"><span data-stu-id="19495-280">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="19495-281">Aşağıdakileri ekleyin `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="19495-281">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="19495-282">İskele `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="19495-282">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="19495-283">Güncelleştirme **ContactManager** içinde yer alan bağlayıcı *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="19495-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="19495-284">İlk geçişin iskelesi ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="19495-284">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="19495-285">Oluşturma, düzenleme ve bir kişi silme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="19495-285">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="19495-286">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="19495-286">Seed the database</span></span>

<span data-ttu-id="19495-287">Ekleme `SeedData` sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="19495-287">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="19495-288">Örnek indirdiğiniz, kopyalayabilirsiniz *SeedData.cs* dosya *veri* başlangıç projesinin klasör.</span><span class="sxs-lookup"><span data-stu-id="19495-288">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="19495-289">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="19495-289">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="19495-290">Uygulama veritabanı sağlanmış sınayın.</span><span class="sxs-lookup"><span data-stu-id="19495-290">Test that the app seeded the database.</span></span> <span data-ttu-id="19495-291">Seed yöntemi kişi DB herhangi bir satır varsa, çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="19495-291">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="19495-292">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="19495-292">Additional resources</span></span>

* <span data-ttu-id="19495-293">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="19495-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="19495-294">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="19495-294">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="19495-295">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="19495-295">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="19495-296">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="19495-296">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
