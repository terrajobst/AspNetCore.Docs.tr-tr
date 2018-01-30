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
ms.openlocfilehash: 944886a7d55af8966dc51424d16bec5ff58dbc05
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="587ab-104">Kullanıcı veri yetkilendirme tarafından korunan bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="587ab-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="587ab-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="587ab-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="587ab-106">Bu öğretici bir ASP.NET Core web uygulamasının yetkilendirme tarafından korunan kullanıcı verileri ile nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="587ab-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="587ab-107">Kimliği doğrulanmış (kayıtlı) kullanıcılar Kişiler listesini görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="587ab-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="587ab-108">Üç güvenlik grupları vardır:</span><span class="sxs-lookup"><span data-stu-id="587ab-108">There are three security groups:</span></span>

* <span data-ttu-id="587ab-109">**Kayıtlı kullanıcıların** tüm onaylanmış verilerini görüntüleyebilir ve düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="587ab-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="587ab-110">**Yöneticileri** onaylayabilir veya kişi verilerini reddedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="587ab-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="587ab-111">Yalnızca onaylanan kişilere kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="587ab-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="587ab-112">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="587ab-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="587ab-113">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) imzalanır.</span><span class="sxs-lookup"><span data-stu-id="587ab-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="587ab-114">Rick yalnızca onaylanan kişilere görüntülemek ve **Düzenle**/**silmek**/**Yeni Oluştur** kendi kişiler için bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="587ab-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="587ab-115">Yalnızca son kaydı görüntüler Rick tarafından oluşturulan **Düzenle** ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="587ab-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="587ab-116">Bir yöneticinin "Onaylandı" durumuna kadar diğer kullanıcılar son kaydı görmez.</span><span class="sxs-lookup"><span data-stu-id="587ab-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Önceki görüntü açıklanan](secure-data/_static/rick.png)

<span data-ttu-id="587ab-118">Aşağıdaki görüntüde `manager@contoso.com` ve yöneticileri rolünde imzalı:</span><span class="sxs-lookup"><span data-stu-id="587ab-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/manager1.png)

<span data-ttu-id="587ab-120">Aşağıdaki resimde yöneticileri Ayrıntılar görünümü bir iletişim gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="587ab-120">The following image shows the managers details view of a contact:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/manager.png)

<span data-ttu-id="587ab-122">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="587ab-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="587ab-123">Aşağıdaki görüntüde `admin@contoso.com` ve yöneticiler rolünde imzalı:</span><span class="sxs-lookup"><span data-stu-id="587ab-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Önceki görüntü açıklanan](secure-data/_static/admin.png)

<span data-ttu-id="587ab-125">Yönetici, tüm ayrıcalıklarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="587ab-125">The administrator has all privileges.</span></span> <span data-ttu-id="587ab-126">Depoladığından, herhangi bir kişiyi okuma/düzenleme/silme yapabilir ve kişiler durumunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="587ab-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="587ab-127">Uygulama tarafından oluşturulan [iskele](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="587ab-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="587ab-128">Örnek aşağıdaki yetkilendirme işleyicileri içerir:</span><span class="sxs-lookup"><span data-stu-id="587ab-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="587ab-129">`ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi veri düzenleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="587ab-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="587ab-130">`ContactManagerAuthorizationHandler`: Onaylamak veya reddetmek kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="587ab-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="587ab-131">`ContactAdministratorsAuthorizationHandler`: Yöneticilerinin onaylama veya reddetme kişiler ve kişi düzenleme/silme sağlar.</span><span class="sxs-lookup"><span data-stu-id="587ab-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="587ab-132">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="587ab-132">Prerequisites</span></span>

<span data-ttu-id="587ab-133">Bu öğretici Gelişmiş.</span><span class="sxs-lookup"><span data-stu-id="587ab-133">This tutorial is advanced.</span></span> <span data-ttu-id="587ab-134">Hakkında bilginiz olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="587ab-134">You should be familiar with:</span></span>

* [<span data-ttu-id="587ab-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="587ab-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="587ab-136">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="587ab-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="587ab-137">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="587ab-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="587ab-138">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="587ab-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="587ab-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="587ab-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="587ab-140">Bu öğretici ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="587ab-140">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="587ab-141">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="587ab-141">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="587ab-142">Tamamlanan uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="587ab-142">The starter and completed app</span></span>

<span data-ttu-id="587ab-143">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="587ab-143">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="587ab-144">[Test](#test-the-completed-app) tamamlanan uygulama ile güvenlik özellikleri hakkında bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="587ab-144">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="587ab-145">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="587ab-145">The starter app</span></span>

<span data-ttu-id="587ab-146">[Karşıdan](xref:tutorials/index#how-to-download-a-sample) [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="587ab-146">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="587ab-147">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="587ab-147">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="587ab-148">Kullanıcı verileri güvenli</span><span class="sxs-lookup"><span data-stu-id="587ab-148">Secure user data</span></span>

<span data-ttu-id="587ab-149">Aşağıdaki bölümlerde güvenli kullanıcı veri uygulaması oluşturmak için tüm önemli adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="587ab-149">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="587ab-150">Tamamlanan projesine başvurması daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="587ab-150">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="587ab-151">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="587ab-151">Tie the contact data to the user</span></span>

<span data-ttu-id="587ab-152">ASP.NET kullanan [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerini, ancak diğer kullanıcılar verileri düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="587ab-152">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="587ab-153">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="587ab-153">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="587ab-154">`OwnerID`kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="587ab-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="587ab-155">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="587ab-155">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="587ab-156">Yeni geçiş oluştur ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="587ab-156">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="587ab-157">SSL ve kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="587ab-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="587ab-158">Ekleme [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) için `Startup`:</span><span class="sxs-lookup"><span data-stu-id="587ab-158">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="587ab-159">İçinde `ConfigureServices` yöntemi *haline* dosya, ekleme [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) yetkilendirme Filtresi:</span><span class="sxs-lookup"><span data-stu-id="587ab-159">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-)]

<span data-ttu-id="587ab-160">Visual Studio kullanıyorsanız, SSL'yi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="587ab-160">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="587ab-161">HTTPS için HTTP isteklerini yeniden yönlendirmek için bkz: [URL yeniden yazma işlemi Ara](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="587ab-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="587ab-162">Visual Studio Code kullanarak ya da yerel bir platformda sınama, SSL için bir test sertifikası içermez:</span><span class="sxs-lookup"><span data-stu-id="587ab-162">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="587ab-163">Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings. Developement.JSON* dosya.</span><span class="sxs-lookup"><span data-stu-id="587ab-163">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="587ab-164">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="587ab-164">Require authenticated users</span></span>

<span data-ttu-id="587ab-165">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="587ab-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="587ab-166">Kimlik doğrulaması ile Razor sayfasını, denetleyici veya eylem yöntemi düzeyinde dışında seçebilirsiniz `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="587ab-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="587ab-167">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfalarının ve denetleyiciler korur.</span><span class="sxs-lookup"><span data-stu-id="587ab-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="587ab-168">Kimlik doğrulaması varsayılan olarak gerekli yeni denetleyicileri ve dahil etmek için Razor sayfalarının bağlı olan daha güvenli olan `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="587ab-168">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="587ab-169">Aşağıdakileri ekleyin `ConfigureServices` yöntemi *haline* dosyası:</span><span class="sxs-lookup"><span data-stu-id="587ab-169">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-)]

<span data-ttu-id="587ab-170">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine, bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri sınıflandırıp hakkında ve iletişim sayfaları.</span><span class="sxs-lookup"><span data-stu-id="587ab-170">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="587ab-171">Ekleme `[AllowAnonymous]` için [LoginModel ve RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="587ab-171">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="587ab-172">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="587ab-172">Configure the test account</span></span>

<span data-ttu-id="587ab-173">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="587ab-173">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="587ab-174">Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="587ab-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="587ab-175">Proje dizinden parola ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="587ab-175">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="587ab-176">Güncelleştirme `Main` test parola kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="587ab-176">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="587ab-177">Test hesapları oluşturabilir ve kişileri güncelleştir</span><span class="sxs-lookup"><span data-stu-id="587ab-177">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="587ab-178">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturmak için sınıfı:</span><span class="sxs-lookup"><span data-stu-id="587ab-178">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="587ab-179">Yönetici kullanıcı kimliği ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="587ab-179">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="587ab-180">"Gönderildi" ve bir "Reddedilen" kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="587ab-180">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="587ab-181">Kullanıcı kimliği ve durum tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="587ab-181">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="587ab-182">Yalnızca bir kişinin gösterilir:</span><span class="sxs-lookup"><span data-stu-id="587ab-182">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="587ab-183">Sahibi, Yöneticisi ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="587ab-183">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="587ab-184">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="587ab-184">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="587ab-185">`ContactIsOwnerAuthorizationHandler` Bir kaynakta hareket kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="587ab-185">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="587ab-186">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) geçerli kimliği doğrulanmış kullanıcı kişi sahibi ise.</span><span class="sxs-lookup"><span data-stu-id="587ab-186">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="587ab-187">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="587ab-187">Authorization handlers generally:</span></span>

* <span data-ttu-id="587ab-188">Dönüş `context.Succeed` zaman gereksinimleri karşılandıktan.</span><span class="sxs-lookup"><span data-stu-id="587ab-188">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="587ab-189">Dönüş `Task.CompletedTask` zaman olmayan gereksinimlerin karşılanması.</span><span class="sxs-lookup"><span data-stu-id="587ab-189">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="587ab-190">`Task.CompletedTask`ne başarılı veya başarısız olduğunu&mdash;çalıştırmak diğer yetkilendirme işleyicileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="587ab-190">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="587ab-191">Açıkça başarısız gerekiyorsa, dönüş [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="587ab-191">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="587ab-192">Uygulamanın kendi veri düzenleme/silme/oluşturmak için kişi sahipleri verir.</span><span class="sxs-lookup"><span data-stu-id="587ab-192">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="587ab-193">`ContactIsOwnerAuthorizationHandler`gereksinim parametrede aktarılan işlemi denetlemek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="587ab-193">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="587ab-194">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="587ab-194">Create a manager authorization handler</span></span>

<span data-ttu-id="587ab-195">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="587ab-195">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="587ab-196">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="587ab-196">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="587ab-197">Yalnızca Yöneticiler onaylayabilir veya reddedebilirsiniz içerik değişikliklerini (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="587ab-197">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="587ab-198">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="587ab-198">Create an administrator authorization handler</span></span>

<span data-ttu-id="587ab-199">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="587ab-199">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="587ab-200">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="587ab-200">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="587ab-201">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="587ab-201">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="587ab-202">Yetkilendirme işleyicileri kaydetme</span><span class="sxs-lookup"><span data-stu-id="587ab-202">Register the authorization handlers</span></span>

<span data-ttu-id="587ab-203">Entity Framework Çekirdek kullanarak Hizmetleri kayıtlı, için [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="587ab-203">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="587ab-204">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="587ab-204">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="587ab-205">Kullanılabilir hizmet koleksiyonuyla işleyicileri kaydolmayı `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="587ab-205">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="587ab-206">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="587ab-206">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-)]

<span data-ttu-id="587ab-207">`ContactAdministratorsAuthorizationHandler`ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="587ab-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="587ab-208">Olup olmadıklarını teklileri EF kullanmayın ve gerekli tüm bilgileri de olduğundan `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="587ab-208">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="587ab-209">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="587ab-209">Support authorization</span></span>

<span data-ttu-id="587ab-210">Bu bölümde, Razor sayfalarının güncelleştirin ve işlemleri gereksinimleri sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="587ab-210">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="587ab-211">Gözden geçirme kişi operations gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="587ab-211">Review the contact operations requirements class</span></span>

<span data-ttu-id="587ab-212">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="587ab-212">Review the `ContactOperations` class.</span></span> <span data-ttu-id="587ab-213">Bu sınıf gereksinimlerini içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="587ab-213">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="587ab-214">Razor sayfalar için temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="587ab-214">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="587ab-215">Kişiler Razor sayfalarının kullanılan hizmetleri içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="587ab-215">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="587ab-216">Taban sınıfı başlatma kodun bir konuma yerleştirir:</span><span class="sxs-lookup"><span data-stu-id="587ab-216">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="587ab-217">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="587ab-217">The preceding code:</span></span>

* <span data-ttu-id="587ab-218">Ekler `IAuthorizationService` yetkilendirme işleyicilerine erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="587ab-218">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="587ab-219">Kimlik ekler `UserManager` hizmet.</span><span class="sxs-lookup"><span data-stu-id="587ab-219">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="587ab-220">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="587ab-220">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="587ab-221">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="587ab-221">Update the CreateModel</span></span>

<span data-ttu-id="587ab-222">Kullanılacak Oluştur sayfası modeli Oluşturucusu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="587ab-222">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="587ab-223">Güncelleştirme `CreateModel.OnPostAsync` yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="587ab-223">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="587ab-224">Kullanıcı kimliği eklemek `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="587ab-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="587ab-225">Kullanıcının kişiler oluşturmak için izne sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="587ab-225">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="587ab-226">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="587ab-226">Update the IndexModel</span></span>

<span data-ttu-id="587ab-227">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilen şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="587ab-227">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="587ab-228">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="587ab-228">Update the EditModel</span></span>

<span data-ttu-id="587ab-229">Kullanıcının kişinin sahip olduğu doğrulamak için bir yetkilendirme işleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="587ab-229">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="587ab-230">Kaynak Yetkilendirme doğrulandığı için `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="587ab-230">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="587ab-231">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="587ab-231">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="587ab-232">Kaynak tabanlı bir yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="587ab-232">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="587ab-233">Uygulama sayfası modelinde yükleme ya da işleyici içinde yükleme kaynağına erişimi olduğunda denetimlerinin gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="587ab-233">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="587ab-234">Sık sık kaynak anahtarı geçirerek kaynak erişir.</span><span class="sxs-lookup"><span data-stu-id="587ab-234">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="587ab-235">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="587ab-235">Update the DeleteModel</span></span>

<span data-ttu-id="587ab-236">Kullanıcı, kişi hakkında silme iznine sahip doğrulamak için yetkilendirme işleyicisi kullanmak için silme sayfa modeli güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="587ab-236">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="587ab-237">Yetkilendirme hizmeti görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="587ab-237">Inject the authorization service into the views</span></span>

<span data-ttu-id="587ab-238">Şu anda, UI gösterir düzenleyebilir ve kullanıcı değiştirilemiyor veri bağlantıları silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="587ab-238">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="587ab-239">Kullanıcı arabirimini görünümlerine yetkilendirme işleyici uygulayarak düzeltilmiştir.</span><span class="sxs-lookup"><span data-stu-id="587ab-239">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="587ab-240">Yetkilendirme hizmetinde Ekle *Views/_ViewImports.cshtml* tüm görünümler kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="587ab-240">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="587ab-241">Önceki biçimlendirme birkaç ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="587ab-241">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="587ab-242">Güncelleştirme **Düzenle** ve **silmek** içinde bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar için işlenen için:</span><span class="sxs-lookup"><span data-stu-id="587ab-242">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-)]

> [!WARNING]
> <span data-ttu-id="587ab-243">Veri değiştirme iznine sahip olmayan kullanıcılardan gelen bağlantılar gizleme uygulama güvenli değil.</span><span class="sxs-lookup"><span data-stu-id="587ab-243">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="587ab-244">Bağlantılar gizleme uygulama daha kullanıcı dostu yalnızca geçerli bağlantılar görüntüleyerek yapmaz.</span><span class="sxs-lookup"><span data-stu-id="587ab-244">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="587ab-245">Kullanıcıları düzenleme çağırma ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'lerini korsan saldırılarına.</span><span class="sxs-lookup"><span data-stu-id="587ab-245">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="587ab-246">Razor sayfasını veya denetleyicisi verileri korumak için erişim denetimleri zorunlu gerekir.</span><span class="sxs-lookup"><span data-stu-id="587ab-246">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="587ab-247">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="587ab-247">Update Details</span></span>

<span data-ttu-id="587ab-248">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="587ab-248">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-)]

<span data-ttu-id="587ab-249">Ayrıntılar sayfası modeli güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="587ab-249">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="587ab-250">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="587ab-250">Test the completed app</span></span>

<span data-ttu-id="587ab-251">Visual Studio Code kullanarak ya da yerel bir platformda sınama, SSL için bir test sertifikası içermez:</span><span class="sxs-lookup"><span data-stu-id="587ab-251">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="587ab-252">Ayarlama `"LocalTest:skipSSL": true` içinde *appsettings. Developement.JSON* SSL gereksinimlerini atlamak için dosya.</span><span class="sxs-lookup"><span data-stu-id="587ab-252">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="587ab-253">SSL yalnızca geliştirme makinenizde atlayın.</span><span class="sxs-lookup"><span data-stu-id="587ab-253">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="587ab-254">Uygulamanın kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="587ab-254">If the app has contacts:</span></span>

* <span data-ttu-id="587ab-255">Tüm kayıtları silin `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="587ab-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="587ab-256">Veritabanını oluşturmak için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="587ab-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="587ab-257">Bir kullanıcı kişiler göz atmak için kaydolun.</span><span class="sxs-lookup"><span data-stu-id="587ab-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="587ab-258">Üç farklı tarayıcılar (veya ıncognito/InPrivate sürümler) başlatmak için tamamlanan uygulama test etmek için kolay bir yol değil.</span><span class="sxs-lookup"><span data-stu-id="587ab-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="587ab-259">Bir tarayıcıda, yeni bir kullanıcı kaydı (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="587ab-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="587ab-260">Her tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="587ab-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="587ab-261">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="587ab-261">Verify the following operations:</span></span>

* <span data-ttu-id="587ab-262">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="587ab-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="587ab-263">Kayıtlı kullanıcıların düzenleme/kullanıcıların kendi verilerini silme.</span><span class="sxs-lookup"><span data-stu-id="587ab-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="587ab-264">Yöneticileri onaylayın veya kişi verilerini reddedin.</span><span class="sxs-lookup"><span data-stu-id="587ab-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="587ab-265">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeler.</span><span class="sxs-lookup"><span data-stu-id="587ab-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="587ab-266">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="587ab-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="587ab-267">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="587ab-267">User</span></span>| <span data-ttu-id="587ab-268">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="587ab-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="587ab-269">Düzenle/kendi veri silmek</span><span class="sxs-lookup"><span data-stu-id="587ab-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="587ab-270">Onayla/Reddet ve düzenleme/silme veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="587ab-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="587ab-271">Düzenleme/silme ve tüm veri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="587ab-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="587ab-272">Bir kişi yöneticinin tarayıcıda oluşturun.</span><span class="sxs-lookup"><span data-stu-id="587ab-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="587ab-273">Delete URL'sini kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="587ab-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="587ab-274">Bu bağlantıları test kullanıcısı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcıya yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="587ab-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="587ab-275">Başlangıç uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="587ab-275">Create the starter app</span></span>

* <span data-ttu-id="587ab-276">"ContactManager" adlı bir Razor sayfalarının uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="587ab-276">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="587ab-277">Uygulama oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="587ab-277">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="587ab-278">Aşağıdaki örnekte kullanılan ad ad alanınızı eşleşecek şekilde "ContactManager" adı.</span><span class="sxs-lookup"><span data-stu-id="587ab-278">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="587ab-279">`-uld`SQLite yerine LocalDB belirtir</span><span class="sxs-lookup"><span data-stu-id="587ab-279">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="587ab-280">Aşağıdakileri ekleyin `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="587ab-280">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="587ab-281">İskele `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="587ab-281">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="587ab-282">Güncelleştirme **ContactManager** içinde yer alan bağlayıcı *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="587ab-282">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="587ab-283">İlk geçişin iskelesi ve veritabanı güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="587ab-283">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="587ab-284">Oluşturma, düzenleme ve bir kişi silme uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="587ab-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="587ab-285">Çekirdek veritabanı</span><span class="sxs-lookup"><span data-stu-id="587ab-285">Seed the database</span></span>

<span data-ttu-id="587ab-286">Ekleme `SeedData` sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="587ab-286">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="587ab-287">Örnek indirdiğiniz, kopyalayabilirsiniz *SeedData.cs* dosya *veri* başlangıç projesinin klasör.</span><span class="sxs-lookup"><span data-stu-id="587ab-287">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="587ab-288">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="587ab-288">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="587ab-289">Uygulama veritabanı sağlanmış sınayın.</span><span class="sxs-lookup"><span data-stu-id="587ab-289">Test that the app seeded the database.</span></span> <span data-ttu-id="587ab-290">Seed yöntemi kişi DB herhangi bir satır varsa, çalıştırmaz.</span><span class="sxs-lookup"><span data-stu-id="587ab-290">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="587ab-291">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="587ab-291">Additional resources</span></span>

* <span data-ttu-id="587ab-292">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="587ab-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="587ab-293">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="587ab-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="587ab-294">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="587ab-294">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="587ab-295">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="587ab-295">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
