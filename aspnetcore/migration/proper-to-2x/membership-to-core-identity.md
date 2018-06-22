---
title: ASP.NET Core 2.0 kimliği için ASP.NET üyelik kimlik doğrulamasını geçirme
author: isaac2004
description: ASP.NET Core 2.0 kimlik üyelik kimlik doğrulaması kullanarak mevcut ASP.NET uygulamaları geçirmek öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3ec22713997a74b587ef5d18e71a28668a5481e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274111"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="478c5-103">ASP.NET Core 2.0 kimliği için ASP.NET üyelik kimlik doğrulamasını geçirme</span><span class="sxs-lookup"><span data-stu-id="478c5-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="478c5-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="478c5-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="478c5-105">Bu makalede, ASP.NET Core 2.0 kimlik üyelik kimlik doğrulaması kullanarak ASP.NET uygulamaları için veritabanı şeması geçirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="478c5-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="478c5-106">Bu belge veritabanı şeması ASP.NET üyelik tabanlı uygulamalar için ASP.NET Core kimliği için kullanılan veritabanı şeması geçirmek için gerekli adımları sağlar.</span><span class="sxs-lookup"><span data-stu-id="478c5-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="478c5-107">ASP.NET üyelik tabanlı kimlik doğrulamasını ASP.NET Identity geçirme hakkında daha fazla bilgi için bkz: [mevcut bir uygulamayı ASP.NET Identity SQL üyeliğinden geçirmek](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="478c5-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="478c5-108">ASP.NET Core kimliği hakkında daha fazla bilgi için bkz: [kimliği ASP.NET Core üzerinde giriş](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="478c5-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="478c5-109">Üyelik şemasının gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="478c5-109">Review of Membership schema</span></span>

<span data-ttu-id="478c5-110">ASP.NET 2.0 önce geliştiriciler uygulamalarını için tüm kimlik doğrulama ve yetkilendirme işlemi oluşturma ile görevli.</span><span class="sxs-lookup"><span data-stu-id="478c5-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="478c5-111">ASP.NET 2.0 ile güvenlik ASP.NET uygulamaların içindeki işleme için ortak bir çözüm sağlayarak üyelik sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="478c5-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="478c5-112">Geliştiriciler ile bir SQL Server veritabanına bir şema bootstrap mümkün şimdi [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) komutu.</span><span class="sxs-lookup"><span data-stu-id="478c5-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="478c5-113">Bu komutu çalıştırdıktan sonra aşağıdaki tablolarda veritabanında oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="478c5-113">After running this command, the following tables were created in the database.</span></span>

  ![Üyelik tablolarını](identity/_static/membership-tables.png)

<span data-ttu-id="478c5-115">ASP.NET Core 2.0 kimliği için var olan uygulamaları geçirmek için bu tablolardaki verileri yeni kimlik şema tarafından kullanılan tablolar için geçirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="478c5-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="478c5-116">ASP.NET Core kimlik 2.0 şeması</span><span class="sxs-lookup"><span data-stu-id="478c5-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="478c5-117">ASP.NET Core 2.0 izleyen [kimlik](/aspnet/identity/index) ASP.NET 4.5 içinde sunulan ilkesi.</span><span class="sxs-lookup"><span data-stu-id="478c5-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="478c5-118">İlkeye paylaşılan olsa çerçeveleri arasında hatta ASP.NET Core sürümleri arasında farklı uygulamasıdır (bkz [ASP.NET Core 2.0 kimlik doğrulama ve kimlik geçirmek](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="478c5-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="478c5-119">ASP.NET Core 2.0 Identity için şema görüntülemek için en hızlı yolu, yeni bir ASP.NET Core 2.0 uygulaması oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="478c5-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="478c5-120">Visual Studio 2017'nda aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="478c5-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="478c5-121">Select **File** > **New** > **Project**.</span><span class="sxs-lookup"><span data-stu-id="478c5-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="478c5-122">Yeni bir **ASP.NET çekirdek Web uygulaması**ve proje adı *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="478c5-122">Create a new **ASP.NET Core Web Application**, and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="478c5-123">Seçin **ASP.NET Core 2.0** açılır ve ardından **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="478c5-123">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span> <span data-ttu-id="478c5-124">Bu şablon üreten bir [Razor sayfalarının](xref:razor-pages/index) uygulama.</span><span class="sxs-lookup"><span data-stu-id="478c5-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="478c5-125">Tıklatmadan önce **Tamam**, tıklatın **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="478c5-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="478c5-126">Seçin **tek tek kullanıcı hesaplarını** kimlik şablonları için.</span><span class="sxs-lookup"><span data-stu-id="478c5-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="478c5-127">Son olarak, tıklatın **Tamam**, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="478c5-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="478c5-128">Visual Studio ASP.NET Core kimliği şablonunu kullanarak bir proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="478c5-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="478c5-129">ASP.NET Core 2.0 kimliğini kullanır [Entity Framework Çekirdek](/ef/core) kimlik doğrulama verileri depolamak veritabanıyla etkileşim kurmak için.</span><span class="sxs-lookup"><span data-stu-id="478c5-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="478c5-130">Yeni oluşturulan uygulamanın çalışması için sırayla var. Bu verileri depolamak için bir veritabanı olması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="478c5-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="478c5-131">Yeni bir uygulama oluşturduktan sonra bir veritabanı ortamında şema incelemek için en hızlı yolu Entity Framework geçişler kullanarak veritabanı oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="478c5-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="478c5-132">Bu işlem, bir veritabanı, yerel olarak ya da başka bir yerde, hangi bu şema taklit eder oluşturur.</span><span class="sxs-lookup"><span data-stu-id="478c5-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="478c5-133">Daha fazla bilgi için önceki belgelerini gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="478c5-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="478c5-134">ASP.NET Core kimliği şeması ile bir veritabanı oluşturmak için çalıştırın `Update-Database` Visual Studio'nun komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi&mdash;konumunda bulunan **Araçları**  >  **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="478c5-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="478c5-135">PMC çalışan Entity Framework komutları destekler.</span><span class="sxs-lookup"><span data-stu-id="478c5-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="478c5-136">Entity Framework komutları belirtilen veritabanı için bağlantı dizesi kullanın *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="478c5-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="478c5-137">Aşağıdaki bağlantı dizesi üzerinde bir veritabanını hedefleyip *localhost* adlı *asp net çekirdek kimlik*.</span><span class="sxs-lookup"><span data-stu-id="478c5-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="478c5-138">Bu ayar, Entity Framework kullanmak üzere yapılandırılmış `DefaultConnection` bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="478c5-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="478c5-139">Bu komut, şemasıyla belirtilen veritabanı ve uygulama başlatma için gereken tüm verileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="478c5-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="478c5-140">Aşağıdaki resimde, yukarıdaki adımları ile oluşturulan tablo yapısı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="478c5-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Kimlik tabloları](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="478c5-142">Şema geçirme</span><span class="sxs-lookup"><span data-stu-id="478c5-142">Migrate the schema</span></span>

<span data-ttu-id="478c5-143">Tablo yapıları ve üyelik ve ASP.NET Core kimlik alanlarında küçük farklılıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="478c5-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="478c5-144">Desen, ASP.NET ve ASP.NET Core uygulamaları ile kimlik doğrulama/yetkilendirme için önemli ölçüde değişti.</span><span class="sxs-lookup"><span data-stu-id="478c5-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="478c5-145">Kimlikle hala kullanılan anahtar nesneler *kullanıcılar* ve *rolleri*.</span><span class="sxs-lookup"><span data-stu-id="478c5-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="478c5-146">İçin eşleme tabloları işte *kullanıcılar*, *rolleri*, ve *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="478c5-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="478c5-147">Kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="478c5-147">Users</span></span>

| <span data-ttu-id="478c5-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="478c5-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="478c5-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="478c5-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="478c5-150">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="478c5-150">**Field Name**</span></span> | <span data-ttu-id="478c5-151">**Türü**</span><span class="sxs-lookup"><span data-stu-id="478c5-151">**Type**</span></span>  |   <span data-ttu-id="478c5-152">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="478c5-152">**Field Name**</span></span> | <span data-ttu-id="478c5-153">**Türü**</span><span class="sxs-lookup"><span data-stu-id="478c5-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="478c5-154">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="478c5-155">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-155">string</span></span>
|`UserName` | <span data-ttu-id="478c5-156">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="478c5-157">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-157">string</span></span>
|`Email` | <span data-ttu-id="478c5-158">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="478c5-159">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="478c5-160">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="478c5-161">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="478c5-162">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="478c5-163">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="478c5-164">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="478c5-165">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="478c5-166">bit</span><span class="sxs-lookup"><span data-stu-id="478c5-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="478c5-167">bit</span><span class="sxs-lookup"><span data-stu-id="478c5-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="478c5-168">Tüm alan eşlemelerini ASP.NET Core kimliği üyeliğinin bire bir ilişkiler benzer.</span><span class="sxs-lookup"><span data-stu-id="478c5-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="478c5-169">Önceki tabloda varsayılan üyelik kullanıcısının şema alır ve ASP.NET Core kimliği şemaya eşler.</span><span class="sxs-lookup"><span data-stu-id="478c5-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="478c5-170">Üyelik için kullanılan herhangi bir özel alanlar el ile eşlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="478c5-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="478c5-171">Bu eşleme içinde yoktur parolalar için hiçbir eşleme parola ölçütlerini ve parola salts ikisi arasında geçirme gibi.</span><span class="sxs-lookup"><span data-stu-id="478c5-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="478c5-172">**Bu parola null olarak bırakın ve kullanıcıların parolalarını sıfırlayabilmesi için sormak için önerilir.**</span><span class="sxs-lookup"><span data-stu-id="478c5-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="478c5-173">ASP.NET çekirdekli Kimlikteki `LockoutEnd` kullanıcıya kilitlenmişse tarihi gelecekte ayarlamanız gerekir. Bu geçiş komut dosyasındaki gösterilir.</span><span class="sxs-lookup"><span data-stu-id="478c5-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="478c5-174">Roller</span><span class="sxs-lookup"><span data-stu-id="478c5-174">Roles</span></span>

| <span data-ttu-id="478c5-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="478c5-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="478c5-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="478c5-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="478c5-177">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="478c5-177">**Field Name**</span></span> | <span data-ttu-id="478c5-178">**Türü**</span><span class="sxs-lookup"><span data-stu-id="478c5-178">**Type**</span></span>  |   <span data-ttu-id="478c5-179">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="478c5-179">**Field Name**</span></span> | <span data-ttu-id="478c5-180">**Türü**</span><span class="sxs-lookup"><span data-stu-id="478c5-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="478c5-181">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-181">string</span></span> | `RoleId` | <span data-ttu-id="478c5-182">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-182">string</span></span>
|`Name` | <span data-ttu-id="478c5-183">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-183">string</span></span> | `RoleName` | <span data-ttu-id="478c5-184">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="478c5-185">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="478c5-186">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="478c5-187">Kullanıcı Rolleri</span><span class="sxs-lookup"><span data-stu-id="478c5-187">User Roles</span></span>

| <span data-ttu-id="478c5-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="478c5-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="478c5-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="478c5-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="478c5-190">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="478c5-190">**Field Name**</span></span> | <span data-ttu-id="478c5-191">**Türü**</span><span class="sxs-lookup"><span data-stu-id="478c5-191">**Type**</span></span>  |   <span data-ttu-id="478c5-192">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="478c5-192">**Field Name**</span></span> | <span data-ttu-id="478c5-193">**Türü**</span><span class="sxs-lookup"><span data-stu-id="478c5-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="478c5-194">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-194">string</span></span> | `RoleId` | <span data-ttu-id="478c5-195">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-195">string</span></span>
|`UserId` | <span data-ttu-id="478c5-196">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-196">string</span></span> | `UserId` | <span data-ttu-id="478c5-197">dize</span><span class="sxs-lookup"><span data-stu-id="478c5-197">string</span></span>

<span data-ttu-id="478c5-198">Başvuru için bir geçiş komut dosyası oluştururken, önceki eşleme tabloları *kullanıcılar* ve *rolleri*.</span><span class="sxs-lookup"><span data-stu-id="478c5-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="478c5-199">Aşağıdaki örnek, bir veritabanı sunucusunda iki veritabanı olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="478c5-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="478c5-200">Bir veritabanı varolan ASP.NET üyelik şemasını ve verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="478c5-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="478c5-201">Daha önce açıklanan adımları kullanarak diğer veritabanı oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="478c5-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="478c5-202">Daha fazla ayrıntı için dahil edilen satır içi açıklamalardır.</span><span class="sxs-lookup"><span data-stu-id="478c5-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="478c5-203">Bu komut dosyası tamamlandıktan sonra daha önce oluşturduğunuz ASP.NET Core kimlik uygulaması üyelik kullanıcılarının ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="478c5-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="478c5-204">Kullanıcıların oturum açmayı önce kullanıcıların parolalarını değiştirmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="478c5-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="478c5-205">Üyelik Sistemi kullanıcılar kendi e-posta adresi ile eşleşmedi kullanıcı adlarıyla olsaydı, bunu uygun hale getirmek için daha önce oluşturduğunuz uygulama değişiklik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="478c5-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="478c5-206">Varsayılan şablonu bekliyor `UserName` ve `Email` aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="478c5-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="478c5-207">Bunlar farklı durumlarda, oturum açma işlemi kullanmak için değiştirilmesi gereken `UserName` yerine `Email`.</span><span class="sxs-lookup"><span data-stu-id="478c5-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="478c5-208">İçinde `PageModel` konumunda bulunan oturum açma sayfasının *Pages\Account\Login.cshtml.cs*, kaldırma `[EmailAddress]` özniteliğini *e-posta* özelliği.</span><span class="sxs-lookup"><span data-stu-id="478c5-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="478c5-209">İçin yeniden adlandırmadan *kullanıcıadı*.</span><span class="sxs-lookup"><span data-stu-id="478c5-209">Rename it to *UserName*.</span></span> <span data-ttu-id="478c5-210">Bu değişiklik gerektiren her yerde `EmailAddress` , içinde açıklanan *Görünüm* ve *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="478c5-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="478c5-211">Sonuç aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="478c5-211">The result looks like the following:</span></span>

 ![Sabit oturum açma](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="478c5-213">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="478c5-213">Next steps</span></span>

<span data-ttu-id="478c5-214">Bu öğreticide, ASP.NET Core 2.0 kimlik SQL üyeliğinin kullanıcılara bağlantı noktası öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="478c5-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="478c5-215">ASP.NET Core kimliği ile ilgili daha fazla bilgi için bkz: [kimlik giriş](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="478c5-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
