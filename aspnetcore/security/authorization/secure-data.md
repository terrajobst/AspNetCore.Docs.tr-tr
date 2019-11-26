---
title: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma
author: rick-anderson
description: Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile Razor sayfaları uygulaması oluşturmayı öğrenin. HTTPS, kimlik doğrulama, güvenlik, ASP.NET Core kimliği içerir.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 65c72d4dd457f85451796c5713bedebafec7a7de
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239827"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="6b15d-104">Kullanıcı verilerinin yetkilendirme tarafından korunduğu ile bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="6b15d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [ali Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="6b15d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="6b15d-106">ASP.NET Core MVC sürümü için [Bu PDF 'ye](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) bakın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="6b15d-107">Bu öğreticinin ASP.NET Core 1,1 sürümü [Bu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) klasöründedir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="6b15d-108">1,1 ASP.NET Core örnek [örnekleri](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="6b15d-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6b15d-109">[Bu PDF 'ye](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf) bakın</span><span class="sxs-lookup"><span data-stu-id="6b15d-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6b15d-110">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="6b15d-111">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="6b15d-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="6b15d-112">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="6b15d-112">There are three security groups:</span></span>

* <span data-ttu-id="6b15d-113">**Kayıtlı kullanıcılar** tüm onaylanan verileri görüntüleyebilir ve kendi verilerini düzenleyebilir/silebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="6b15d-114">**Yöneticiler** , kişi verilerini onaylayabilir veya reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="6b15d-115">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="6b15d-116">**Yöneticiler** , verileri onaylayabilir/reddedebilir ve düzenleyebilir/silebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="6b15d-117">Bu belgedeki görüntüler en son şablonlarla tam olarak eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="6b15d-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="6b15d-118">Aşağıdaki görüntüde, User Rick (`rick@example.com`) oturum açtı.</span><span class="sxs-lookup"><span data-stu-id="6b15d-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="6b15d-119">Rick, onaylanan kişileri görüntüleyebilir ve kişiler için **yeni bağlantılar oluşturmak**//**silebilir** ve **düzenleyebilir** .</span><span class="sxs-lookup"><span data-stu-id="6b15d-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="6b15d-120">Yalnızca Rick tarafından oluşturulan son kayıt, **Düzenle** ve **Sil** bağlantılarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="6b15d-121">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="6b15d-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

<span data-ttu-id="6b15d-123">Aşağıdaki görüntüde `manager@contoso.com`, ve yöneticisinin rolünde oturum açtı:</span><span class="sxs-lookup"><span data-stu-id="6b15d-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![manager@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/manager1.png)

<span data-ttu-id="6b15d-125">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-125">The following image shows the managers details view of a contact:</span></span>

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

<span data-ttu-id="6b15d-127">**Onaylama** ve **reddetme** düğmeleri yalnızca Yöneticiler ve yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="6b15d-128">Aşağıdaki görüntüde `admin@contoso.com`, ve yönetici rolünde oturum açtı:</span><span class="sxs-lookup"><span data-stu-id="6b15d-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![admin@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/admin.png)

<span data-ttu-id="6b15d-130">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="6b15d-130">The administrator has all privileges.</span></span> <span data-ttu-id="6b15d-131">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="6b15d-132">Uygulama, aşağıdaki `Contact` modeli için [Yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) tarafından oluşturulmuştur:</span><span class="sxs-lookup"><span data-stu-id="6b15d-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="6b15d-133">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="6b15d-134">`ContactIsOwnerAuthorizationHandler`: kullanıcının yalnızca verilerini düzenleyebilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="6b15d-135">`ContactManagerAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="6b15d-136">`ContactAdministratorsAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini ve kişileri düzenlemesini/silmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b15d-137">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6b15d-137">Prerequisites</span></span>

<span data-ttu-id="6b15d-138">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-138">This tutorial is advanced.</span></span> <span data-ttu-id="6b15d-139">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="6b15d-139">You should be familiar with:</span></span>

* [<span data-ttu-id="6b15d-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b15d-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6b15d-141">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="6b15d-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="6b15d-142">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="6b15d-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6b15d-143">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="6b15d-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6b15d-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="6b15d-145">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="6b15d-145">The starter and completed app</span></span>

<span data-ttu-id="6b15d-146">[Tamamlanmış](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) uygulamayı [indirin](xref:index#how-to-download-a-sample) .</span><span class="sxs-lookup"><span data-stu-id="6b15d-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="6b15d-147">Tamamlanmış uygulamayı [Test](#test-the-completed-app) edin, böylece güvenlik özellikleri hakkında bilgi sahibi olun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="6b15d-148">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="6b15d-148">The starter app</span></span>

<span data-ttu-id="6b15d-149">[Başlangıç](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) uygulamasını [indirin](xref:index#how-to-download-a-sample) .</span><span class="sxs-lookup"><span data-stu-id="6b15d-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="6b15d-150">Uygulamayı çalıştırın, **ContactManager** bağlantısına dokunun ve bir kişi oluşturup silemdiğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="6b15d-151">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="6b15d-151">Secure user data</span></span>

<span data-ttu-id="6b15d-152">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="6b15d-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="6b15d-153">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="6b15d-154">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="6b15d-154">Tie the contact data to the user</span></span>

<span data-ttu-id="6b15d-155">Kullanıcıların verilerini düzenleyebilmeleri, ancak diğer kullanıcıların verilerini düzenleyebilmeleri için ASP.NET [Identity](xref:security/authentication/identity) Kullanıcı kimliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="6b15d-156">`Contact` modeline `OwnerID` ve `ContactStatus` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="6b15d-157">[kimlik](xref:security/authentication/identity) veritabanındaki `AspNetUser` TABLOSUNDAN kullanıcının kimliği `OwnerID`.</span><span class="sxs-lookup"><span data-stu-id="6b15d-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="6b15d-158">`Status` alanı, bir kişinin genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="6b15d-159">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="6b15d-160">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="6b15d-160">Add Role services to Identity</span></span>

<span data-ttu-id="6b15d-161">Rol hizmetleri eklemek için [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="6b15d-162">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="6b15d-162">Require authenticated users</span></span>

<span data-ttu-id="6b15d-163">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="6b15d-164">`[AllowAnonymous]` özniteliğiyle Razor sayfasında, denetleyicide veya eylem yöntemi düzeyinde kimlik doğrulamasının devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b15d-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="6b15d-165">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="6b15d-166">Varsayılan olarak kimlik doğrulamanın gerekli olması, yeni denetleyicilere bağlı olandan daha güvenlidir ve `[Authorize]` özniteliğini eklemek Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b15d-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="6b15d-167">Anonim kullanıcıların, kaydolmadan önce site hakkında bilgi alması için dizin ve gizlilik sayfalarına [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="6b15d-168">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="6b15d-168">Configure the test account</span></span>

<span data-ttu-id="6b15d-169">`SeedData` sınıfı iki hesap oluşturur: yönetici ve yönetici.</span><span class="sxs-lookup"><span data-stu-id="6b15d-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="6b15d-170">Bu hesaplara bir parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets) kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="6b15d-171">Parolayı proje dizininden ayarlayın ( *program.cs*içeren dizin):</span><span class="sxs-lookup"><span data-stu-id="6b15d-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="6b15d-172">Güçlü bir parola belirtilmemişse `SeedData.Initialize` çağrıldığında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="6b15d-173">Sınama parolasını kullanmak için `Main` güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="6b15d-174">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="6b15d-175">Test hesapları oluşturmak için `SeedData` sınıfındaki `Initialize` yöntemi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="6b15d-176">Yönetici kullanıcı KIMLIĞINI ve `ContactStatus` kişilere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="6b15d-177">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="6b15d-178">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="6b15d-179">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="6b15d-180">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="6b15d-181">*Yetkilendirme* klasöründe bir `ContactIsOwnerAuthorizationHandler` sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6b15d-182">`ContactIsOwnerAuthorizationHandler`, bir kaynağın kaynak üzerinde davranan kullanıcının kaynağın sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6b15d-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="6b15d-183">`ContactIsOwnerAuthorizationHandler` bağlamını çağırır [. ](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)Kimliği doğrulanmış geçerli kullanıcı kişi sahibsiyse başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="6b15d-184">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="6b15d-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="6b15d-185">Gereksinimler karşılandığında `context.Succeed` döndürün.</span><span class="sxs-lookup"><span data-stu-id="6b15d-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="6b15d-186">Gereksinimler karşılanmazsa `Task.CompletedTask` döndürün.</span><span class="sxs-lookup"><span data-stu-id="6b15d-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="6b15d-187">`Task.CompletedTask`, diğer yetkilendirme işleyicilerinin çalışmasına izin veren&mdash;başarılı veya başarısız değildir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="6b15d-188">Açıkça başarısız olması gerekiyorsa, [bağlamı döndürün. Başarısız oldu](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="6b15d-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="6b15d-189">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="6b15d-190">`ContactIsOwnerAuthorizationHandler` gereksinim parametresine geçirilen işlemi denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6b15d-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="6b15d-191">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-191">Create a manager authorization handler</span></span>

<span data-ttu-id="6b15d-192">*Yetkilendirme* klasöründe bir `ContactManagerAuthorizationHandler` sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6b15d-193">`ContactManagerAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6b15d-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="6b15d-194">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="6b15d-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="6b15d-195">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="6b15d-196">*Yetkilendirme* klasöründe bir `ContactAdministratorsAuthorizationHandler` sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6b15d-197">`ContactAdministratorsAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6b15d-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="6b15d-198">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="6b15d-199">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="6b15d-199">Register the authorization handlers</span></span>

<span data-ttu-id="6b15d-200">Entity Framework Core kullanan hizmetlerin, [Addkapsamlıdır](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)kullanılarak [bağımlılık ekleme](xref:fundamentals/dependency-injection) için kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="6b15d-201">`ContactIsOwnerAuthorizationHandler`, Entity Framework Core oluşturulan ASP.NET Core [kimliğini](xref:security/authentication/identity)kullanır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="6b15d-202">İşleyicileri, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla `ContactsController` için kullanılabilir olmaları için hizmet koleksiyonuyla kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6b15d-203">`ConfigureServices`sonuna aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="6b15d-204">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` tekton olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="6b15d-205">Bunlar, EF kullanmadığından ve gereken tüm bilgiler `HandleRequirementAsync` yönteminin `Context` parametresinde olduğundan tektonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="6b15d-206">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-206">Support authorization</span></span>

<span data-ttu-id="6b15d-207">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="6b15d-208">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="6b15d-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="6b15d-209">`ContactOperations` sınıfını gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="6b15d-210">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="6b15d-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="6b15d-211">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="6b15d-212">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="6b15d-213">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="6b15d-214">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="6b15d-214">The preceding code:</span></span>

* <span data-ttu-id="6b15d-215">Yetkilendirme işleyicilerine erişim için `IAuthorizationService` hizmetini ekler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="6b15d-216">Kimlik `UserManager` hizmetini ekler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="6b15d-217">`ApplicationDbContext`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="6b15d-218">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-218">Update the CreateModel</span></span>

<span data-ttu-id="6b15d-219">Create Page model oluşturucusunu `DI_BasePageModel` temel sınıfını kullanacak şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="6b15d-220">`CreateModel.OnPostAsync` yöntemini şu şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="6b15d-221">Kullanıcı KIMLIĞINI `Contact` modeline ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="6b15d-222">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="6b15d-223">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-223">Update the IndexModel</span></span>

<span data-ttu-id="6b15d-224">`OnGetAsync` yöntemini yalnızca onaylanan kişilerin genel kullanıcılara gösterilmesi için güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="6b15d-225">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-225">Update the EditModel</span></span>

<span data-ttu-id="6b15d-226">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="6b15d-227">Kaynak yetkilendirmesi doğrulanamadığından `[Authorize]` özniteliği yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="6b15d-228">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="6b15d-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="6b15d-229">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="6b15d-230">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="6b15d-231">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="6b15d-232">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-232">Update the DeleteModel</span></span>

<span data-ttu-id="6b15d-233">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="6b15d-234">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="6b15d-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="6b15d-235">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="6b15d-236">Yetkilendirme hizmetini *Sayfalar/_ViewImports. cshtml* dosyasına ekleme, bu nedenle tüm görünümlerde kullanılabilir hale gelir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="6b15d-237">Yukarıdaki biçimlendirme birkaç `using` deyimi ekler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="6b15d-238">*Sayfalar/kişiler/Index. cshtml* 'deki **Düzenle** ve **Sil** bağlantılarını yalnızca uygun izinlere sahip kullanıcılar için işlendikleri şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="6b15d-239">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="6b15d-240">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="6b15d-241">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="6b15d-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="6b15d-242">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="6b15d-243">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="6b15d-243">Update Details</span></span>

<span data-ttu-id="6b15d-244">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="6b15d-245">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="6b15d-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="6b15d-246">Ekleme veya bir rolü için bir kullanıcıyı kaldırma</span><span class="sxs-lookup"><span data-stu-id="6b15d-246">Add or remove a user to a role</span></span>

<span data-ttu-id="6b15d-247">Hakkında bilgi için [Bu soruna](https://github.com/aspnet/AspNetCore.Docs/issues/8502) bakın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="6b15d-248">Bir kullanıcıdan ayrıcalıklarının kaldırılması.</span><span class="sxs-lookup"><span data-stu-id="6b15d-248">Removing privileges from a user.</span></span> <span data-ttu-id="6b15d-249">Örneğin, bir sohbet uygulamasındaki kullanıcıyı kapatma.</span><span class="sxs-lookup"><span data-stu-id="6b15d-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="6b15d-250">Bir kullanıcı ayrıcalıkları ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="6b15d-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="6b15d-251">Challenge ve Fordeklarasyon arasındaki farklar</span><span class="sxs-lookup"><span data-stu-id="6b15d-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="6b15d-252">Bu uygulama, varsayılan ilkeyi [kimliği doğrulanmış kullanıcıları gerektirecek](#require-authenticated-users)şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="6b15d-253">Aşağıdaki kod anonim kullanıcılara izin verir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-253">The following code allows anonymous users.</span></span> <span data-ttu-id="6b15d-254">Anonim kullanıcılara, çekişme ile Fordeklarasyon arasındaki farkları gösterme izni verilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="6b15d-255">Önceki kodda:</span><span class="sxs-lookup"><span data-stu-id="6b15d-255">In the preceding code:</span></span>

* <span data-ttu-id="6b15d-256">Kullanıcının kimliği **doğrulanmadığı** zaman, bir `ChallengeResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="6b15d-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="6b15d-257">Bir `ChallengeResult` döndürüldüğünde, Kullanıcı oturum açma sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="6b15d-258">Kullanıcının kimliği doğrulandığında, ancak yetkilendirilmediğinde, bir `ForbidResult` döndürülür.</span><span class="sxs-lookup"><span data-stu-id="6b15d-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="6b15d-259">Bir `ForbidResult` döndürüldüğünde, Kullanıcı erişim reddedildi sayfasına yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="6b15d-260">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6b15d-260">Test the completed app</span></span>

<span data-ttu-id="6b15d-261">Önceden oluşturulmuş kullanıcı hesapları için bir parola ayarlamadıysanız, parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) kullanın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="6b15d-262">Güçlü bir parola seçin: kullanım sekiz veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge.</span><span class="sxs-lookup"><span data-stu-id="6b15d-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="6b15d-263">Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="6b15d-264">Projenin klasöründen aşağıdaki komutu yürütün, burada `<PW>` paroladır:</span><span class="sxs-lookup"><span data-stu-id="6b15d-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="6b15d-265">Uygulamanın, kişiler varsa:</span><span class="sxs-lookup"><span data-stu-id="6b15d-265">If the app has contacts:</span></span>

* <span data-ttu-id="6b15d-266">`Contact` tablosundaki tüm kayıtları silin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="6b15d-267">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="6b15d-268">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="6b15d-269">Tek bir tarayıcıda yeni bir Kullanıcı kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="6b15d-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="6b15d-270">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="6b15d-271">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-271">Verify the following operations:</span></span>

* <span data-ttu-id="6b15d-272">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="6b15d-273">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="6b15d-274">Yöneticileri Onayla/kişi verilerini reddet.</span><span class="sxs-lookup"><span data-stu-id="6b15d-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="6b15d-275">`Details` görünümü **onaylama** ve **reddetme** düğmelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="6b15d-276">Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="6b15d-277">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="6b15d-277">User</span></span>                | <span data-ttu-id="6b15d-278">Uygulama tarafından sağlanmış</span><span class="sxs-lookup"><span data-stu-id="6b15d-278">Seeded by the app</span></span> | <span data-ttu-id="6b15d-279">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="6b15d-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="6b15d-280">Hayır</span><span class="sxs-lookup"><span data-stu-id="6b15d-280">No</span></span>                | <span data-ttu-id="6b15d-281">Kendi veri düzenleme veya silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="6b15d-282">Evet</span><span class="sxs-lookup"><span data-stu-id="6b15d-282">Yes</span></span>               | <span data-ttu-id="6b15d-283">Onayla/Reddet ve kendi veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="6b15d-284">Evet</span><span class="sxs-lookup"><span data-stu-id="6b15d-284">Yes</span></span>               | <span data-ttu-id="6b15d-285">Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="6b15d-286">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="6b15d-287">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="6b15d-288">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="6b15d-289">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-289">Create the starter app</span></span>

* <span data-ttu-id="6b15d-290">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="6b15d-291">**Ayrı kullanıcı hesaplarıyla**uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="6b15d-292">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="6b15d-293">`-uld`, SQLite yerine LocalDB belirtir</span><span class="sxs-lookup"><span data-stu-id="6b15d-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="6b15d-294">*Modeller/Ilgili kişi ekle. cs*:</span><span class="sxs-lookup"><span data-stu-id="6b15d-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="6b15d-295">`Contact` modeli için yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="6b15d-296">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="6b15d-297">`dotnet aspnet-codegenerator razorpage` komutuyla bir hata yaşarsanız, [Bu GitHub sorununa](https://github.com/aspnet/Scaffolding/issues/984)bakın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="6b15d-298">*Pages/Shared/_Layout. cshtml* dosyasındaki **ContactManager** bağlayıcısını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="6b15d-299">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6b15d-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="6b15d-300">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-300">Seed the database</span></span>

<span data-ttu-id="6b15d-301">*Veri* klasörüne [seeddata](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) sınıfını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-301">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="6b15d-302">`Main``SeedData.Initialize` çağrısı:</span><span class="sxs-lookup"><span data-stu-id="6b15d-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="6b15d-303">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="6b15d-303">Test that the app seeded the database.</span></span> <span data-ttu-id="6b15d-304">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="6b15d-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="6b15d-305">Bu öğretici, kullanıcı verilerinin yetkilendirme tarafından korunduğu ile ASP.NET Core web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="6b15d-306">Kimlik doğrulaması (kayıtlı) kullanıcılar kişiler listesi görüntüler oluşturdunuz.</span><span class="sxs-lookup"><span data-stu-id="6b15d-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="6b15d-307">Üç güvenlik grubu vardır:</span><span class="sxs-lookup"><span data-stu-id="6b15d-307">There are three security groups:</span></span>

* <span data-ttu-id="6b15d-308">**Kayıtlı kullanıcılar** tüm onaylanan verileri görüntüleyebilir ve kendi verilerini düzenleyebilir/silebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="6b15d-309">**Yöneticiler** , kişi verilerini onaylayabilir veya reddedebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="6b15d-310">Yalnızca onaylanan kişilere, kullanıcılar tarafından görülebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="6b15d-311">**Yöneticiler** , verileri onaylayabilir/reddedebilir ve düzenleyebilir/silebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="6b15d-312">Aşağıdaki görüntüde, User Rick (`rick@example.com`) oturum açtı.</span><span class="sxs-lookup"><span data-stu-id="6b15d-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="6b15d-313">Rick, onaylanan kişileri görüntüleyebilir ve kişiler için **yeni bağlantılar oluşturmak**//**silebilir** ve **düzenleyebilir** .</span><span class="sxs-lookup"><span data-stu-id="6b15d-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="6b15d-314">Yalnızca Rick tarafından oluşturulan son kayıt, **Düzenle** ve **Sil** bağlantılarını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="6b15d-315">Bir yöneticinin "Onaylandı" durum olana kadar diğer kullanıcılar en son kaydını görmez.</span><span class="sxs-lookup"><span data-stu-id="6b15d-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Oturum Rick gösteren ekran görüntüsü](secure-data/_static/rick.png)

<span data-ttu-id="6b15d-317">Aşağıdaki görüntüde `manager@contoso.com`, ve yöneticisinin rolünde oturum açtı:</span><span class="sxs-lookup"><span data-stu-id="6b15d-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![manager@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/manager1.png)

<span data-ttu-id="6b15d-319">Aşağıdaki resimde, bir kişi ayrıntıları görünümünü yöneticileri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-319">The following image shows the managers details view of a contact:</span></span>

![Bir kişinin yöneticisinin görüntüle](secure-data/_static/manager.png)

<span data-ttu-id="6b15d-321">**Onaylama** ve **reddetme** düğmeleri yalnızca Yöneticiler ve yöneticiler için görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="6b15d-322">Aşağıdaki görüntüde `admin@contoso.com`, ve yönetici rolünde oturum açtı:</span><span class="sxs-lookup"><span data-stu-id="6b15d-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![admin@contoso.com oturum açmış olduğunu gösteren ekran görüntüsü](secure-data/_static/admin.png)

<span data-ttu-id="6b15d-324">Yöneticisinin tüm ayrıcalıklara sahip değil.</span><span class="sxs-lookup"><span data-stu-id="6b15d-324">The administrator has all privileges.</span></span> <span data-ttu-id="6b15d-325">Okuma/düzenleme/silme herhangi iletişime geçebiliriz ve kişiler birinin durumunu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="6b15d-326">Uygulama, aşağıdaki `Contact` modeli için [Yapı iskelesi](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) tarafından oluşturulmuştur:</span><span class="sxs-lookup"><span data-stu-id="6b15d-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="6b15d-327">Örnek aşağıdaki yetkilendirme işleyicilerini içerir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="6b15d-328">`ContactIsOwnerAuthorizationHandler`: kullanıcının yalnızca verilerini düzenleyebilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="6b15d-329">`ContactManagerAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="6b15d-330">`ContactAdministratorsAuthorizationHandler`: yöneticilerin kişileri onaylamasını veya reddetmesini ve kişileri düzenlemesini/silmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b15d-331">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6b15d-331">Prerequisites</span></span>

<span data-ttu-id="6b15d-332">Bu öğreticide gelişmiştir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-332">This tutorial is advanced.</span></span> <span data-ttu-id="6b15d-333">Sahibi olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="6b15d-333">You should be familiar with:</span></span>

* [<span data-ttu-id="6b15d-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b15d-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6b15d-335">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="6b15d-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="6b15d-336">Hesap Onaylama ve Parola Kurtarma</span><span class="sxs-lookup"><span data-stu-id="6b15d-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6b15d-337">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="6b15d-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6b15d-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="6b15d-339">Tamamlanmış uygulama ve başlangıç</span><span class="sxs-lookup"><span data-stu-id="6b15d-339">The starter and completed app</span></span>

<span data-ttu-id="6b15d-340">[Tamamlanmış](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) uygulamayı [indirin](xref:index#how-to-download-a-sample) .</span><span class="sxs-lookup"><span data-stu-id="6b15d-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="6b15d-341">Tamamlanmış uygulamayı [Test](#test-the-completed-app) edin, böylece güvenlik özellikleri hakkında bilgi sahibi olun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="6b15d-342">Başlangıç uygulaması</span><span class="sxs-lookup"><span data-stu-id="6b15d-342">The starter app</span></span>

<span data-ttu-id="6b15d-343">[Başlangıç](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) uygulamasını [indirin](xref:index#how-to-download-a-sample) .</span><span class="sxs-lookup"><span data-stu-id="6b15d-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="6b15d-344">Uygulamayı çalıştırın, **ContactManager** bağlantısına dokunun ve bir kişi oluşturup silemdiğinizi doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="6b15d-345">Kullanıcı verilerinin güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="6b15d-345">Secure user data</span></span>

<span data-ttu-id="6b15d-346">Aşağıdaki bölümlerde, güvenli kullanıcı veri uygulaması oluşturmak için ana tüm adımları tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="6b15d-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="6b15d-347">Tamamlanan projeye başvuran daha yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="6b15d-348">Kullanıcı kişi verilerini bağlayın</span><span class="sxs-lookup"><span data-stu-id="6b15d-348">Tie the contact data to the user</span></span>

<span data-ttu-id="6b15d-349">Kullanıcıların verilerini düzenleyebilmeleri, ancak diğer kullanıcıların verilerini düzenleyebilmeleri için ASP.NET [Identity](xref:security/authentication/identity) Kullanıcı kimliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="6b15d-350">`Contact` modeline `OwnerID` ve `ContactStatus` ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="6b15d-351">[kimlik](xref:security/authentication/identity) veritabanındaki `AspNetUser` TABLOSUNDAN kullanıcının kimliği `OwnerID`.</span><span class="sxs-lookup"><span data-stu-id="6b15d-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="6b15d-352">`Status` alanı, bir kişinin genel kullanıcılar tarafından görüntülenebilir olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="6b15d-353">Yeni geçiş oluştur ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="6b15d-354">Kimlik için rol hizmetleri Ekle</span><span class="sxs-lookup"><span data-stu-id="6b15d-354">Add Role services to Identity</span></span>

<span data-ttu-id="6b15d-355">Rol hizmetleri eklemek için [Addroles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="6b15d-356">Kimliği doğrulanmış kullanıcılar gerektirir</span><span class="sxs-lookup"><span data-stu-id="6b15d-356">Require authenticated users</span></span>

<span data-ttu-id="6b15d-357">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama ilkesi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="6b15d-358">`[AllowAnonymous]` özniteliğiyle Razor sayfasında, denetleyicide veya eylem yöntemi düzeyinde kimlik doğrulamasının devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b15d-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="6b15d-359">Kullanıcıların kimliklerinin doğrulanmasını istemek için varsayılan kimlik doğrulama İlkesi ayarı, yeni eklenen Razor sayfaları ve denetleyicileri korur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="6b15d-360">Varsayılan olarak kimlik doğrulamanın gerekli olması, yeni denetleyicilere bağlı olandan daha güvenlidir ve `[Authorize]` özniteliğini eklemek Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b15d-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="6b15d-361">Anonim kullanıcıların, kayıt yaptırmadan önce site hakkında bilgi alması için dizine, hakkında ve Iletişim sayfalarına [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="6b15d-362">Test hesabı yapılandırın</span><span class="sxs-lookup"><span data-stu-id="6b15d-362">Configure the test account</span></span>

<span data-ttu-id="6b15d-363">`SeedData` sınıfı iki hesap oluşturur: yönetici ve yönetici.</span><span class="sxs-lookup"><span data-stu-id="6b15d-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="6b15d-364">Bu hesaplara bir parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets) kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="6b15d-365">Parolayı proje dizininden ayarlayın ( *program.cs*içeren dizin):</span><span class="sxs-lookup"><span data-stu-id="6b15d-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="6b15d-366">Güçlü bir parola belirtilmemişse `SeedData.Initialize` çağrıldığında bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="6b15d-367">Sınama parolasını kullanmak için `Main` güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="6b15d-368">Test hesapları oluşturabilir ve kişilerini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="6b15d-369">Test hesapları oluşturmak için `SeedData` sınıfındaki `Initialize` yöntemi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="6b15d-370">Yönetici kullanıcı KIMLIĞINI ve `ContactStatus` kişilere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="6b15d-371">"Gönderildi" ve "Reddedildi" bir kişilerden biri olun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="6b15d-372">Kullanıcı kimliği ve durumu tüm kişileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="6b15d-373">Tek bir kişi gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="6b15d-374">Sahip, yönetici ve yönetici yetkilendirme işleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="6b15d-375">Bir *Yetkilendirme* klasörü oluşturun ve içinde bir `ContactIsOwnerAuthorizationHandler` sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="6b15d-376">`ContactIsOwnerAuthorizationHandler`, bir kaynağın kaynak üzerinde davranan kullanıcının kaynağın sahip olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6b15d-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="6b15d-377">`ContactIsOwnerAuthorizationHandler` bağlamını çağırır [. ](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)Kimliği doğrulanmış geçerli kullanıcı kişi sahibsiyse başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="6b15d-378">Yetkilendirme işleyicileri genellikle:</span><span class="sxs-lookup"><span data-stu-id="6b15d-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="6b15d-379">Gereksinimler karşılandığında `context.Succeed` döndürün.</span><span class="sxs-lookup"><span data-stu-id="6b15d-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="6b15d-380">Gereksinimler karşılanmazsa `Task.CompletedTask` döndürün.</span><span class="sxs-lookup"><span data-stu-id="6b15d-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="6b15d-381">`Task.CompletedTask`, diğer yetkilendirme işleyicilerinin çalışmasına izin veren&mdash;başarılı veya başarısız değildir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="6b15d-382">Açıkça başarısız olması gerekiyorsa, [bağlamı döndürün. Başarısız oldu](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="6b15d-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="6b15d-383">Düzenleme/silme/oluşturma iletişim sahiplere kendi verilerini verir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="6b15d-384">`ContactIsOwnerAuthorizationHandler` gereksinim parametresine geçirilen işlemi denetlemesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6b15d-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="6b15d-385">Bir yönetici yetkilendirme işleyici oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-385">Create a manager authorization handler</span></span>

<span data-ttu-id="6b15d-386">*Yetkilendirme* klasöründe bir `ContactManagerAuthorizationHandler` sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6b15d-387">`ContactManagerAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6b15d-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="6b15d-388">Yalnızca Yöneticiler onaylayabileceğiniz veya reddedebileceğiniz içerik değişiklikleri (yeni veya değiştirilmiş).</span><span class="sxs-lookup"><span data-stu-id="6b15d-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="6b15d-389">Bir yönetici yetkilendirme işleyicisi oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="6b15d-390">*Yetkilendirme* klasöründe bir `ContactAdministratorsAuthorizationHandler` sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="6b15d-391">`ContactAdministratorsAuthorizationHandler`, kaynak üzerinde davranan kullanıcının bir yönetici olduğunu doğrular.</span><span class="sxs-lookup"><span data-stu-id="6b15d-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="6b15d-392">Yönetici, tüm işlemleri yapabilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="6b15d-393">Yetkilendirme işleyicilerini kaydedin</span><span class="sxs-lookup"><span data-stu-id="6b15d-393">Register the authorization handlers</span></span>

<span data-ttu-id="6b15d-394">Entity Framework Core kullanan hizmetlerin, [Addkapsamlıdır](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)kullanılarak [bağımlılık ekleme](xref:fundamentals/dependency-injection) için kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="6b15d-395">`ContactIsOwnerAuthorizationHandler`, Entity Framework Core oluşturulan ASP.NET Core [kimliğini](xref:security/authentication/identity)kullanır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="6b15d-396">İşleyicileri, [bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla `ContactsController` için kullanılabilir olmaları için hizmet koleksiyonuyla kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6b15d-397">`ConfigureServices`sonuna aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="6b15d-398">`ContactAdministratorsAuthorizationHandler` ve `ContactManagerAuthorizationHandler` tekton olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="6b15d-399">Bunlar, EF kullanmadığından ve gereken tüm bilgiler `HandleRequirementAsync` yönteminin `Context` parametresinde olduğundan tektonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="6b15d-400">Destek yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-400">Support authorization</span></span>

<span data-ttu-id="6b15d-401">Bu bölümde, Razor sayfaları güncelleştirmeniz ve bir işlem gereksinimleri sınıfı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="6b15d-402">Gözden geçirme kişi işlem gereksinimleri sınıfı</span><span class="sxs-lookup"><span data-stu-id="6b15d-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="6b15d-403">`ContactOperations` sınıfını gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="6b15d-404">Bu sınıf gereksinimleri içeren uygulama destekler:</span><span class="sxs-lookup"><span data-stu-id="6b15d-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="6b15d-405">Kişiler Razor sayfaları için bir temel sınıf oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="6b15d-406">Razor sayfaları kişiler kullanılan hizmetler içeren temel bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="6b15d-407">Temel sınıf başlatma kodunu tek bir yerde getirir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="6b15d-408">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="6b15d-408">The preceding code:</span></span>

* <span data-ttu-id="6b15d-409">Yetkilendirme işleyicilerine erişim için `IAuthorizationService` hizmetini ekler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="6b15d-410">Kimlik `UserManager` hizmetini ekler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="6b15d-411">`ApplicationDbContext`ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="6b15d-412">Güncelleştirme CreateModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-412">Update the CreateModel</span></span>

<span data-ttu-id="6b15d-413">Create Page model oluşturucusunu `DI_BasePageModel` temel sınıfını kullanacak şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="6b15d-414">`CreateModel.OnPostAsync` yöntemini şu şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="6b15d-415">Kullanıcı KIMLIĞINI `Contact` modeline ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="6b15d-416">Kullanıcının Kişiler oluşturma iznine sahip doğrulamak için yetkilendirme işleyici çağırın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="6b15d-417">Güncelleştirme IndexModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-417">Update the IndexModel</span></span>

<span data-ttu-id="6b15d-418">`OnGetAsync` yöntemini yalnızca onaylanan kişilerin genel kullanıcılara gösterilmesi için güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="6b15d-419">Güncelleştirme EditModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-419">Update the EditModel</span></span>

<span data-ttu-id="6b15d-420">Kullanıcının sahip olduğu kişi doğrulamak için bir yetkilendirme işleyicisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="6b15d-421">Kaynak yetkilendirmesi doğrulanamadığından `[Authorize]` özniteliği yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="6b15d-422">Öznitelikleri değerlendirildiğinde uygulama kaynağa erişimi yok.</span><span class="sxs-lookup"><span data-stu-id="6b15d-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="6b15d-423">Kaynak tabanlı yetkilendirme kesinlik temelli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="6b15d-424">Uygulama sayfası modele yükleniyor ya da işleyicisinin içerisinde yükleme kaynağına erişimi olduğunda denetimleri gerçekleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="6b15d-425">Sık sık geçirerek kaynak anahtarı kaynağa erişir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="6b15d-426">Güncelleştirme DeleteModel</span><span class="sxs-lookup"><span data-stu-id="6b15d-426">Update the DeleteModel</span></span>

<span data-ttu-id="6b15d-427">Delete sayfa modeli, kişi hakkında kullanıcıyı silme izni doğrulamak için yetkilendirme işleyicisi kullanacak şekilde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="6b15d-428">Kimlik doğrulama servisi görünümlere ekleme</span><span class="sxs-lookup"><span data-stu-id="6b15d-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="6b15d-429">Şu anda kullanıcı Arabirimi gösterir düzenleyin ve kişiler kullanıcı değiştiremez bağlantılarını silin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="6b15d-430">Yetkilendirme hizmetini *Görünümler/_ViewImports. cshtml* dosyasında, tüm görünümlerde kullanılabilir olacak şekilde ekleme:</span><span class="sxs-lookup"><span data-stu-id="6b15d-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="6b15d-431">Yukarıdaki biçimlendirme birkaç `using` deyimi ekler.</span><span class="sxs-lookup"><span data-stu-id="6b15d-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="6b15d-432">*Sayfalar/kişiler/Index. cshtml* 'deki **Düzenle** ve **Sil** bağlantılarını yalnızca uygun izinlere sahip kullanıcılar için işlendikleri şekilde güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="6b15d-433">Bağlantıları veri değiştirme iznine sahip olmayan kullanıcılardan gizleyerek uygulama güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="6b15d-434">Bağlantıları gizleme uygulamayı daha kullanıcı dostu yalnızca geçerli bağlantıları görüntüleyerek yapar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="6b15d-435">Kullanıcıları düzenleme çağırmak ve silme işlemleri ait olmayan veriler üzerinde oluşturulan URL'leri hack.</span><span class="sxs-lookup"><span data-stu-id="6b15d-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="6b15d-436">Razor sayfası veya denetleyici verileri korumak için erişim denetimleri zorlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="6b15d-437">Güncelleştirme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="6b15d-437">Update Details</span></span>

<span data-ttu-id="6b15d-438">Ayrıntılar görünümü yöneticileri onaylayabilir veya reddedebilir kişiler güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="6b15d-439">Güncelleştirme ayrıntıları sayfa modeli:</span><span class="sxs-lookup"><span data-stu-id="6b15d-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="6b15d-440">Ekleme veya bir rolü için bir kullanıcıyı kaldırma</span><span class="sxs-lookup"><span data-stu-id="6b15d-440">Add or remove a user to a role</span></span>

<span data-ttu-id="6b15d-441">Hakkında bilgi için [Bu soruna](https://github.com/aspnet/AspNetCore.Docs/issues/8502) bakın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-441">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="6b15d-442">Bir kullanıcıdan ayrıcalıklarının kaldırılması.</span><span class="sxs-lookup"><span data-stu-id="6b15d-442">Removing privileges from a user.</span></span> <span data-ttu-id="6b15d-443">Örneğin, bir sohbet uygulamasındaki kullanıcıyı kapatma.</span><span class="sxs-lookup"><span data-stu-id="6b15d-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="6b15d-444">Bir kullanıcı ayrıcalıkları ekleniyor.</span><span class="sxs-lookup"><span data-stu-id="6b15d-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="6b15d-445">Tamamlanmış uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6b15d-445">Test the completed app</span></span>

<span data-ttu-id="6b15d-446">Önceden oluşturulmuş kullanıcı hesapları için bir parola ayarlamadıysanız, parola ayarlamak için [gizli dizi Yöneticisi aracını](xref:security/app-secrets#secret-manager) kullanın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="6b15d-447">Güçlü bir parola seçin: kullanım sekiz veya daha fazla karakter ve en az bir büyük harf karakter, sayı ve simge.</span><span class="sxs-lookup"><span data-stu-id="6b15d-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="6b15d-448">Örneğin, `Passw0rd!` güçlü parola gereksinimlerini karşılar.</span><span class="sxs-lookup"><span data-stu-id="6b15d-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="6b15d-449">Projenin klasöründen aşağıdaki komutu yürütün, burada `<PW>` paroladır:</span><span class="sxs-lookup"><span data-stu-id="6b15d-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="6b15d-450">Veritabanını bırakma ve güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="6b15d-451">Veritabanının çekirdeğini oluşturma için uygulamayı yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="6b15d-452">Tamamlanmış uygulamayı test etmek için kolay bir yolu, üç farklı tarayıcılar (veya ıncognito/InPrivate oturumları) başlatılmasıdır.</span><span class="sxs-lookup"><span data-stu-id="6b15d-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="6b15d-453">Tek bir tarayıcıda yeni bir Kullanıcı kaydedin (örneğin, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="6b15d-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="6b15d-454">Her bir tarayıcıya farklı bir kullanıcı ile oturum açın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="6b15d-455">Aşağıdaki işlemleri doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="6b15d-455">Verify the following operations:</span></span>

* <span data-ttu-id="6b15d-456">Kayıtlı kullanıcılar tüm onaylanmış kişi verilerini görüntüleyebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="6b15d-457">Kayıtlı kullanıcıların kendi verilerini düzenleme/silebilir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="6b15d-458">Yöneticileri Onayla/kişi verilerini reddet.</span><span class="sxs-lookup"><span data-stu-id="6b15d-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="6b15d-459">`Details` görünümü **onaylama** ve **reddetme** düğmelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="6b15d-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="6b15d-460">Yöneticiler Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="6b15d-461">Kullanıcı</span><span class="sxs-lookup"><span data-stu-id="6b15d-461">User</span></span>                | <span data-ttu-id="6b15d-462">Uygulama tarafından sağlanmış</span><span class="sxs-lookup"><span data-stu-id="6b15d-462">Seeded by the app</span></span> | <span data-ttu-id="6b15d-463">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="6b15d-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="6b15d-464">Hayır</span><span class="sxs-lookup"><span data-stu-id="6b15d-464">No</span></span>                | <span data-ttu-id="6b15d-465">Kendi veri düzenleme veya silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="6b15d-466">Evet</span><span class="sxs-lookup"><span data-stu-id="6b15d-466">Yes</span></span>               | <span data-ttu-id="6b15d-467">Onayla/Reddet ve kendi veri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="6b15d-468">Evet</span><span class="sxs-lookup"><span data-stu-id="6b15d-468">Yes</span></span>               | <span data-ttu-id="6b15d-469">Onayla/Reddet ve tüm verileri düzenleme/silme.</span><span class="sxs-lookup"><span data-stu-id="6b15d-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="6b15d-470">Bir kişi, yöneticinin tarayıcıda oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6b15d-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="6b15d-471">Silme için URL'yi kopyalayın ve yönetici kişiden düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="6b15d-472">Bu bağlantıları test kullanıcı bu işlemleri gerçekleştiremezsiniz doğrulamak için test kullanıcının tarayıcısına yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="6b15d-473">Başlangıç uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-473">Create the starter app</span></span>

* <span data-ttu-id="6b15d-474">"ContactManager" adlı bir Razor sayfaları uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="6b15d-475">**Ayrı kullanıcı hesaplarıyla**uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6b15d-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="6b15d-476">Örnekte kullanılan ad alanı ad alanı eşleşecek şekilde "ContactManager" adlandırın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="6b15d-477">`-uld`, SQLite yerine LocalDB belirtir</span><span class="sxs-lookup"><span data-stu-id="6b15d-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="6b15d-478">*Modeller/Ilgili kişi ekle. cs*:</span><span class="sxs-lookup"><span data-stu-id="6b15d-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="6b15d-479">`Contact` modeli için yapı iskelesi yapın.</span><span class="sxs-lookup"><span data-stu-id="6b15d-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="6b15d-480">İlk geçiş oluşturun ve veritabanını güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="6b15d-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="6b15d-481">*Pages/_Layout. cshtml* dosyasındaki **ContactManager** bağlayıcısını güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="6b15d-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="6b15d-482">Oluşturma, düzenleme ve silme kişi uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="6b15d-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="6b15d-483">Veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b15d-483">Seed the database</span></span>

<span data-ttu-id="6b15d-484">*Veri* klasörüne [seeddata](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) sınıfını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6b15d-484">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="6b15d-485">`Main``SeedData.Initialize` çağrısı:</span><span class="sxs-lookup"><span data-stu-id="6b15d-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="6b15d-486">Test, uygulama veritabanını sağlanmış.</span><span class="sxs-lookup"><span data-stu-id="6b15d-486">Test that the app seeded the database.</span></span> <span data-ttu-id="6b15d-487">DB ilgili herhangi bir satır varsa, seed yöntemi çalıştırılmaz.</span><span class="sxs-lookup"><span data-stu-id="6b15d-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="6b15d-488">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6b15d-488">Additional resources</span></span>

* [<span data-ttu-id="6b15d-489">Azure App Service bir .NET Core ve SQL veritabanı Web uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="6b15d-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="6b15d-490">[ASP.NET Core yetkilendirme Laboratuvarı](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="6b15d-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="6b15d-491">Bu Laboratuvar, bu öğreticide sunulan güvenlik özellikleri hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="6b15d-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="6b15d-492">Özel ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="6b15d-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
