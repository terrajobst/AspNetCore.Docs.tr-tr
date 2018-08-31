---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 9f264daa4a6b63478077cadb06a697f274014199
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336017"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="bffda-104">Bkz: [bu PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC sürümü için.</span><span class="sxs-lookup"><span data-stu-id="bffda-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="bffda-105">Bu öğreticide ASP.NET Core 1.1 sürümü [bu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) klasör.</span><span class="sxs-lookup"><span data-stu-id="bffda-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="bffda-106">ASP.NET Core örnek konusu 1.1 [örnekleri](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="bffda-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bffda-107">Bkz: [bu pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="bffda-107">See [this pdf](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="bffda-108">Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="bffda-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="bffda-109">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ALi Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="bffda-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="bffda-110">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bffda-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="bffda-111">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="bffda-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="bffda-112">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="bffda-112">There are three security groups:</span></span>

* <span data-ttu-id="bffda-113">**Kayıtlı kullanıcıların** onaylanan tüm verileri görüntüleyebilir ve kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="bffda-114">**Yöneticileri** onaylayabilecek veya reddedebilecek kişi verilerini.</span><span class="sxs-lookup"><span data-stu-id="bffda-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="bffda-115">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="bffda-116">**Yöneticiler** Onayla/Reddet ve herhangi bir veri düzenleme/silme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bffda-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="bffda-117">Aşağıdaki görüntüde, kullanıcı Rick (`rick@example.com`) açmıştır.</span><span class="sxs-lookup"><span data-stu-id="bffda-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="bffda-118">Rick yalnızca onaylanan kişiler görüntüleyebilir ve **Düzenle**/**Sil**/**Yeni Oluştur** kendi kişiler için bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="bffda-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="bffda-119">Yalnızca en son kaydını görüntüler Rick tarafından oluşturulan **Düzenle** ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="bffda-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="bffda-120">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="bffda-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/rick.png)

<span data-ttu-id="bffda-122">Aşağıdaki görüntüde, `manager@contoso.com` ve yöneticileri rolünde imzalanır:</span><span class="sxs-lookup"><span data-stu-id="bffda-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/manager1.png)

<span data-ttu-id="bffda-124">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="bffda-124">The following image shows the managers details view of a contact:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/manager.png)

<span data-ttu-id="bffda-126">**Onayla** ve **Reddet** düğmeleri yalnızca Yöneticiler ve Yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bffda-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="bffda-127">Aşağıdaki görüntüde, `admin@contoso.com` ve yöneticiler rolünde imzalanır:</span><span class="sxs-lookup"><span data-stu-id="bffda-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Görüntü, önceki açıklanan](secure-data/_static/admin.png)

<span data-ttu-id="bffda-129">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="bffda-129">The administrator has all privileges.</span></span> <span data-ttu-id="bffda-130">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bffda-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="bffda-131">Uygulama tarafından oluşturulan [yapı iskelesi](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) aşağıdaki `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="bffda-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="bffda-132">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="bffda-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="bffda-133">`ContactIsOwnerAuthorizationHandler`: Bir kullanıcı yalnızca kendi verilerini düzenleyebilir sağlar.</span><span class="sxs-lookup"><span data-stu-id="bffda-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="bffda-134">`ContactManagerAuthorizationHandler`: Onaylama veya reddetme kişiler yöneticileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="bffda-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="bffda-135">`ContactAdministratorsAuthorizationHandler`: Yöneticilerin onaylama veya reddetme kişiler ve kişiler düzenleme/silme olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bffda-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bffda-136">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="bffda-136">Prerequisites</span></span>

<span data-ttu-id="bffda-137">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="bffda-137">This tutorial is advanced.</span></span> <span data-ttu-id="bffda-138">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="bffda-138">You should be familiar with:</span></span>

* [<span data-ttu-id="bffda-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bffda-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="bffda-140">Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="bffda-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="bffda-141">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="bffda-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="bffda-142">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="bffda-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="bffda-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="bffda-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end
::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="bffda-144">ASP.NET Core 2.1 içinde `User.IsInRole` kullanırken başarısız `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="bffda-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="bffda-145">Bu öğreticide `AddDefaultIdentity` ve bu nedenle ASP.NET Core 2.2 Önizleme 1 veya üzerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bffda-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="bffda-146">Bkz: [bu GitHub sorunu](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) geçici için.</span><span class="sxs-lookup"><span data-stu-id="bffda-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="bffda-147">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="bffda-147">The starter and completed app</span></span>

<span data-ttu-id="bffda-148">[İndirme](xref:tutorials/index#how-to-download-a-sample) [tamamlandı](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="bffda-148">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="bffda-149">[Test](#test-the-completed-app) tamamlanan uygulama güvenlik özellikleri ile ilgili bilgi sahibi olursunuz.</span><span class="sxs-lookup"><span data-stu-id="bffda-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="bffda-150">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="bffda-150">The starter app</span></span>

<span data-ttu-id="bffda-151">[İndirme](xref:tutorials/index#how-to-download-a-sample) [başlangıç](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) uygulama.</span><span class="sxs-lookup"><span data-stu-id="bffda-151">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="bffda-152">Uygulamayı çalıştırın, dokunun **ContactManager** bağlamak ve oluşturmak, düzenlemek ve bir kişi Sil doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bffda-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="bffda-153">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="bffda-153">Secure user data</span></span>

<span data-ttu-id="bffda-154">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="bffda-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="bffda-155">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="bffda-156">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="bffda-156">Tie the contact data to the user</span></span>

<span data-ttu-id="bffda-157">ASP.NET kullanmak [kimlik](xref:security/authentication/identity) kullanıcılar emin olmak için kullanıcı kimliği verilerine, ancak diğer kullanıcıların verileri düzenleyebilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="bffda-158">Ekleme `OwnerID` ve `ContactStatus` için `Contact` modeli:</span><span class="sxs-lookup"><span data-stu-id="bffda-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="bffda-159">`OwnerID` kullanıcının kimliği olan `AspNetUser` tablosundaki [kimlik](xref:security/authentication/identity) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="bffda-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="bffda-160">`Status` Alanı, bir kişi genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="bffda-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="bffda-161">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="bffda-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="bffda-162">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="bffda-162">Add Role services to Identity</span></span>

<span data-ttu-id="bffda-163">Append [ndeki](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) rol hizmetlerini eklemek için:</span><span class="sxs-lookup"><span data-stu-id="bffda-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="bffda-164">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="bffda-164">Require authenticated users</span></span>

<span data-ttu-id="bffda-165">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="bffda-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="bffda-166">Kimlik doğrulaması ile Razor sayfası, denetleyici veya eylem yöntemi düzeyinde dışında iyileştirilmiş `[AllowAnonymous]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="bffda-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="bffda-167">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="bffda-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="bffda-168">Kimlik doğrulaması varsayılan olarak gerekli olan bağlı olan, yeni denetleyicileri ve dahil etmek için Razor sayfaları hakkında daha fazla güvenli `[Authorize]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="bffda-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="bffda-169">Ekleme [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) dizine hakkında ve kişi sayfaları böylece bunlar kaydetmeden önce anonim kullanıcılar siteyle ilgili bilgileri elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bffda-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="bffda-170">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="bffda-170">Configure the test account</span></span>

<span data-ttu-id="bffda-171">`SeedData` Sınıfı iki hesap oluşturur: Yönetici ve.</span><span class="sxs-lookup"><span data-stu-id="bffda-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="bffda-172">Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) bu hesaplar için bir parola ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="bffda-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="bffda-173">Proje dizininden parolayı ayarlayın (dizinini içeren *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="bffda-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="bffda-174">Güçlü bir parola belirtilmediği takdirde, bir özel durum `SeedData.Initialize` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bffda-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="bffda-175">Güncelleştirme `Main` test parola kullanmak üzere:</span><span class="sxs-lookup"><span data-stu-id="bffda-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="bffda-176">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="bffda-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="bffda-177">Güncelleştirme `Initialize` yönteminde `SeedData` test hesapları oluşturma sınıfı:</span><span class="sxs-lookup"><span data-stu-id="bffda-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="bffda-178">Yönetici kullanıcı Kimliğini ekleyin ve `ContactStatus` kişilere.</span><span class="sxs-lookup"><span data-stu-id="bffda-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="bffda-179">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="bffda-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="bffda-180">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bffda-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="bffda-181">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="bffda-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="bffda-182">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="bffda-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="bffda-183">Oluşturma bir `ContactIsOwnerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="bffda-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="bffda-184">`ContactIsOwnerAuthorizationHandler` Bir kaynakta çalışan kullanıcı kaynağın sahibi doğrular.</span><span class="sxs-lookup"><span data-stu-id="bffda-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="bffda-185">`ContactIsOwnerAuthorizationHandler` Çağrıları [bağlamı. Başarılı](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) kişi sahibi kimliği doğrulanmış geçerli kullanıcı ise.</span><span class="sxs-lookup"><span data-stu-id="bffda-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="bffda-186">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="bffda-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="bffda-187">Dönüş `context.Succeed` gereksinimleri karşılanmadığı zaman.</span><span class="sxs-lookup"><span data-stu-id="bffda-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="bffda-188">Dönüş `Task.CompletedTask` zaman olmayan gereksinimleri karşılanıyor.</span><span class="sxs-lookup"><span data-stu-id="bffda-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="bffda-189">`Task.CompletedTask` ne success veya FAILURE olur&mdash;çalıştırmak, diğer yetkilendirme işleyicileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="bffda-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="bffda-190">Açıkça başarısız gerekiyorsa, iade [bağlamı. Başarısız](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="bffda-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="bffda-191">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="bffda-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="bffda-192">`ContactIsOwnerAuthorizationHandler` gereksinim parametrede aktarılan işlemi denetleyin. gerekmez.</span><span class="sxs-lookup"><span data-stu-id="bffda-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="bffda-193">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="bffda-193">Create a manager authorization handler</span></span>

<span data-ttu-id="bffda-194">Oluşturma bir `ContactManagerAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="bffda-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="bffda-195">`ContactManagerAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, bir yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="bffda-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="bffda-196">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="bffda-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="bffda-197">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="bffda-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="bffda-198">Oluşturma bir `ContactAdministratorsAuthorizationHandler` sınıfını *yetkilendirme* klasör.</span><span class="sxs-lookup"><span data-stu-id="bffda-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="bffda-199">`ContactAdministratorsAuthorizationHandler` Kaynak üzerinde çalışan kullanıcı, yönetici yer almadığını doğrular.</span><span class="sxs-lookup"><span data-stu-id="bffda-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="bffda-200">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="bffda-201">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="bffda-201">Register the authorization handlers</span></span>

<span data-ttu-id="bffda-202">Entity Framework Core kullanan hizmetler için kaydedilmelidir [bağımlılık ekleme](xref:fundamentals/dependency-injection) kullanarak [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="bffda-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="bffda-203">`ContactIsOwnerAuthorizationHandler` ASP.NET Core kullanan [kimlik](xref:security/authentication/identity), Entity Framework Core üzerinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bffda-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="bffda-204">Kullanılabilir bulunmaları işleyicilerini hizmet koleksiyonuyla kaydedin `ContactsController` aracılığıyla [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bffda-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bffda-205">Sonuna aşağıdaki kodu ekleyin `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bffda-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="bffda-206">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` teklileri eklenir.</span><span class="sxs-lookup"><span data-stu-id="bffda-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="bffda-207">Oldukları teklileri çünkü bunlar EF kullanmayın ve gereken tüm bilgiler `Context` parametresinin `HandleRequirementAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bffda-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="bffda-208">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="bffda-208">Support authorization</span></span>

<span data-ttu-id="bffda-209">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bffda-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="bffda-210">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="bffda-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="bffda-211">Gözden geçirme `ContactOperations` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bffda-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="bffda-212">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="bffda-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="bffda-213">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="bffda-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="bffda-214">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bffda-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="bffda-215">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="bffda-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="bffda-216">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="bffda-216">The preceding code:</span></span>

* <span data-ttu-id="bffda-217">Ekler `IAuthorizationService` yetkilendirme işleyicilere erişmek için hizmet.</span><span class="sxs-lookup"><span data-stu-id="bffda-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="bffda-218">Kimlik ekler `UserManager` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="bffda-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="bffda-219">Ekleme `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="bffda-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="bffda-220">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="bffda-220">Update the CreateModel</span></span>

<span data-ttu-id="bffda-221">Kullanılacak Oluştur sayfası model Oluşturucu güncelleştirme `DI_BasePageModel` temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="bffda-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="bffda-222">Güncelleştirme `CreateModel.OnPostAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bffda-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="bffda-223">Kullanıcı Kimliğinize ekleme `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="bffda-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="bffda-224">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="bffda-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="bffda-225">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="bffda-225">Update the IndexModel</span></span>

<span data-ttu-id="bffda-226">Güncelleştirme `OnGetAsync` yalnızca onaylanan kişilere genel kullanıcılara gösterilecek şekilde yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bffda-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="bffda-227">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="bffda-227">Update the EditModel</span></span>

<span data-ttu-id="bffda-228">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bffda-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="bffda-229">Kaynak Yetkilendirme Doğrulanmakta olan çünkü `[Authorize]` özniteliği yeterli değil.</span><span class="sxs-lookup"><span data-stu-id="bffda-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="bffda-230">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="bffda-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="bffda-231">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bffda-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="bffda-232">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="bffda-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="bffda-233">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="bffda-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="bffda-234">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="bffda-234">Update the DeleteModel</span></span>

<span data-ttu-id="bffda-235">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="bffda-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="bffda-236">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="bffda-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="bffda-237">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="bffda-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="bffda-238">Yetkilendirme hizmetinde ekleme *Views/_ViewImports.cshtml* tüm görünümlere kullanılabilir olacak şekilde dosya:</span><span class="sxs-lookup"><span data-stu-id="bffda-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="bffda-239">Birkaç önceki biçimlendirme ekler `using` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="bffda-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="bffda-240">Güncelleştirme **Düzenle** ve **Sil** bağlantılar *Pages/Contacts/Index.cshtml* bunlar yalnızca uygun izinlere sahip kullanıcılar tarafından işlenen için:</span><span class="sxs-lookup"><span data-stu-id="bffda-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="bffda-241">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="bffda-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="bffda-242">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="bffda-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="bffda-243">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="bffda-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="bffda-244">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bffda-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="bffda-245">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="bffda-245">Update Details</span></span>

<span data-ttu-id="bffda-246">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="bffda-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="bffda-247">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="bffda-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="bffda-248">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="bffda-248">Test the completed app</span></span>

<span data-ttu-id="bffda-249">Uygulamanın, kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="bffda-249">If the app has contacts:</span></span>

* <span data-ttu-id="bffda-250">Tüm kayıtları silme `Contact` tablo.</span><span class="sxs-lookup"><span data-stu-id="bffda-250">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="bffda-251">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="bffda-251">Restart the app to seed the database.</span></span>

<span data-ttu-id="bffda-252">Bir kullanıcı kişiler göz atmak için kaydedin.</span><span class="sxs-lookup"><span data-stu-id="bffda-252">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="bffda-253">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate sürümleri) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="bffda-253">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="bffda-254">Yeni bir kullanıcı bir tarayıcıda kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="bffda-254">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="bffda-255">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="bffda-255">Sign in to each browser with a different user.</span></span> <span data-ttu-id="bffda-256">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="bffda-256">Verify the following operations:</span></span>

* <span data-ttu-id="bffda-257">Kayıtlı kullanıcılar, tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-257">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="bffda-258">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="bffda-258">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="bffda-259">Yöneticileri onaylayabilir veya kişi verilerinizi reddedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bffda-259">Managers can approve or reject contact data.</span></span> <span data-ttu-id="bffda-260">`Details` Görüntülemek gösterir **Onayla** ve **Reddet** düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="bffda-260">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="bffda-261">Yöneticiler Onayla/Reddet ve herhangi bir veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="bffda-261">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="bffda-262">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="bffda-262">User</span></span>| <span data-ttu-id="bffda-263">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="bffda-263">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="bffda-264">Kendi veri düzenleme/silebilir</span><span class="sxs-lookup"><span data-stu-id="bffda-264">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="bffda-265">Onayla/Reddet ve düzenleme/silme, veri sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="bffda-265">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="bffda-266">Düzenleme/silme ve tüm verileri Onayla/Reddet kullanabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="bffda-266">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="bffda-267">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bffda-267">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="bffda-268">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="bffda-268">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="bffda-269">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="bffda-269">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="bffda-270">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="bffda-270">Create the starter app</span></span>

* <span data-ttu-id="bffda-271">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="bffda-271">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="bffda-272">Uygulamayı oluşturma **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="bffda-272">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="bffda-273">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="bffda-273">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="bffda-274">`-uld` LocalDB yerine SQLite belirtir</span><span class="sxs-lookup"><span data-stu-id="bffda-274">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="bffda-275">Ekleme *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="bffda-275">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="bffda-276">İskele `Contact` modeli.</span><span class="sxs-lookup"><span data-stu-id="bffda-276">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="bffda-277">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="bffda-277">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="bffda-278">Güncelleştirme **ContactManager** içinde yer işareti *Pages/_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="bffda-278">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="bffda-279">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="bffda-279">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="bffda-280">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="bffda-280">Seed the database</span></span>

<span data-ttu-id="bffda-281">Ekleme [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfının *veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="bffda-281">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="bffda-282">Çağrı `SeedData.Initialize` gelen `Main`:</span><span class="sxs-lookup"><span data-stu-id="bffda-282">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="bffda-283">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="bffda-283">Test that the app seeded the database.</span></span> <span data-ttu-id="bffda-284">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="bffda-284">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="bffda-285">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bffda-285">Additional resources</span></span>

* <span data-ttu-id="bffda-286">[ASP.NET Core yetkilendirme Laboratuvar](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="bffda-286">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="bffda-287">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="bffda-287">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="bffda-288">ASP.NET Core yetkilendirme: basit, rol, talep tabanlı ve özel</span><span class="sxs-lookup"><span data-stu-id="bffda-288">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="bffda-289">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="bffda-289">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
