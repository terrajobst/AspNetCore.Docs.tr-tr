---
title: ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme
author: isaac2004
description: ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak varolan ASP.NET uygulamalarını geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41753137"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="bfdeb-103">ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="bfdeb-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="bfdeb-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="bfdeb-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="bfdeb-105">Bu makalede, ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak ASP.NET uygulamaları için veritabanı şemasını geçirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="bfdeb-106">Bu belge, veritabanı şemasını ASP.NET üyelik tabanlı uygulamalar için ASP.NET Core kimliği için kullanılan veritabanı şemasını geçirmek için gerekli olan adımları sağlar.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="bfdeb-107">ASP.NET üyelik tabanlı kimlik doğrulamasını ASP.NET Identity'ye geçirme hakkında daha fazla bilgi için bkz. [mevcut bir uygulamayı SQL üyeliğinden ASP.NET Identity'ye geçirme](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="bfdeb-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="bfdeb-108">ASP.NET Core kimliği hakkında daha fazla bilgi için bkz: [ASP.NET Core kimliği giriş](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="bfdeb-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="bfdeb-109">Üyelik şeması gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="bfdeb-109">Review of Membership schema</span></span>

<span data-ttu-id="bfdeb-110">ASP.NET 2.0 önce geliştiricilerin uygulamalarını tüm kimlik doğrulama ve yetkilendirme işlemi oluşturmaya görevli.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="bfdeb-111">ASP.NET 2.0 ile ASP.NET uygulamaları içinde güvenlik işlemek için ortak bir çözüm sağlayarak üyelik sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="bfdeb-112">Geliştiriciler ile bir SQL Server veritabanına bir şema bootstrap için artık [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) komutu.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="bfdeb-113">Bu komutu çalıştırdıktan sonra aşağıdaki tablolarda veritabanında oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-113">After running this command, the following tables were created in the database.</span></span>

  ![Üyelik tablolarını](identity/_static/membership-tables.png)

<span data-ttu-id="bfdeb-115">ASP.NET Core 2.0 kimliği için mevcut uygulamaları geçirmek için bu tablolardaki verilerin yeni kimlik şema tarafından kullanılan tablolar için geçirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="bfdeb-116">ASP.NET Core kimlik 2.0 şeması</span><span class="sxs-lookup"><span data-stu-id="bfdeb-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="bfdeb-117">ASP.NET Core 2.0 izleyen [kimlik](/aspnet/identity/index) ASP.NET 4.5 içinde tanıtılan ilkesi.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="bfdeb-118">Uygulama çerçeveleri arasında ilkesini paylaşılan olsa bile ASP.NET Core sürümleri arasında farklı (bkz [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="bfdeb-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="bfdeb-119">ASP.NET Core 2.0 kimliği için şemasını görüntülemek için en hızlı yolu, yeni bir ASP.NET Core 2.0 uygulaması oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="bfdeb-120">Visual Studio 2017'de şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="bfdeb-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="bfdeb-121">Seçin **dosya** > **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="bfdeb-122">Yeni bir **ASP.NET Core Web uygulaması** ve projeyi adlandırın *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-122">Create a new **ASP.NET Core Web Application** and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="bfdeb-123">Seçin **ASP.NET Core 2.0** seçin ve açılan **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="bfdeb-124">Bu şablon üreten bir [Razor sayfaları](xref:razor-pages/index) uygulama.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="bfdeb-125">Tıklatmadan önce **Tamam**, tıklayın **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="bfdeb-126">Seçin **bireysel kullanıcı hesapları** kimlik şablonları.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="bfdeb-127">Son olarak, tıklayın **Tamam**, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="bfdeb-128">Visual Studio, ASP.NET Core kimliği şablonunu kullanarak bir proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="bfdeb-129">ASP.NET Core 2.0 kullanan kimlik [Entity Framework Core](/ef/core) kimlik doğrulama verileri depolamak veritabanıyla etkileşime geçmek için.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="bfdeb-130">İçin yeni oluşturulan bir uygulamanın çalışması için sırayla var. Bu verileri depolamak için bir veritabanı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="bfdeb-131">Yeni bir uygulama oluşturduktan sonra en hızlı yolu, bir veritabanı ortam içinde şema incelemek için Entity Framework geçişleri kullanarak veritabanı oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="bfdeb-132">Bu işlem, bu şema taklit eden bir veritabanı, yerel olarak veya başka bir yerde, oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="bfdeb-133">Daha fazla bilgi için yukarıdaki belgelerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="bfdeb-134">Bir veritabanı ile ASP.NET Core kimliği şema oluşturmak için çalıştırma `Update-Database` Visual Studio'nun komutunu **Paket Yöneticisi Konsolu** (PMC) penceresi&mdash;konumunda bulunan **Araçları**  >  **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="bfdeb-135">PMC, Entity Framework komutlarını çalıştırmayı destekliyor.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="bfdeb-136">Entity Framework komutları belirtilen veritabanı için bağlantı dizesini kullanın *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="bfdeb-137">Bir veritabanı bağlantı dizesi hedefleyen *localhost* adlı *asp net core kimliği*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="bfdeb-138">Bu ayarda, Entity Framework kullanmak üzere yapılandırılmış `DefaultConnection` bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="bfdeb-139">Bu komut, şema ile belirtilen veritabanı ve uygulama başlatma için gereken tüm verileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="bfdeb-140">Aşağıdaki görüntüde ile önceki adımlarda oluşturulan tablo yapısı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Kimlik tabloları](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="bfdeb-142">Geçiş şeması</span><span class="sxs-lookup"><span data-stu-id="bfdeb-142">Migrate the schema</span></span>

<span data-ttu-id="bfdeb-143">Tablo yapıları ve alanları üyelik hem de ASP.NET Core kimliği için küçük farklılıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="bfdeb-144">Desen, ASP.NET ve ASP.NET Core uygulamaları ile kimlik doğrulama/yetkilendirme için önemli ölçüde değişti.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="bfdeb-145">Kimlikle hala kullanılan anahtar nesneler *kullanıcılar* ve *rolleri*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="bfdeb-146">Eşleme tablolar için işte *kullanıcılar*, *rolleri*, ve *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="bfdeb-147">Kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="bfdeb-147">Users</span></span>

| <span data-ttu-id="bfdeb-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="bfdeb-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="bfdeb-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="bfdeb-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bfdeb-150">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-150">**Field Name**</span></span> | <span data-ttu-id="bfdeb-151">**Türü**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-151">**Type**</span></span>  |   <span data-ttu-id="bfdeb-152">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-152">**Field Name**</span></span> | <span data-ttu-id="bfdeb-153">**Türü**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="bfdeb-154">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="bfdeb-155">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-155">string</span></span>
|`UserName` | <span data-ttu-id="bfdeb-156">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="bfdeb-157">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-157">string</span></span>
|`Email` | <span data-ttu-id="bfdeb-158">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="bfdeb-159">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="bfdeb-160">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="bfdeb-161">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="bfdeb-162">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="bfdeb-163">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="bfdeb-164">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="bfdeb-165">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="bfdeb-166">bit</span><span class="sxs-lookup"><span data-stu-id="bfdeb-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="bfdeb-167">bit</span><span class="sxs-lookup"><span data-stu-id="bfdeb-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="bfdeb-168">Tüm alan eşlemelerini üyeliğinin bire bir ilişkiler ASP.NET Core kimliği için benzer.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="bfdeb-169">Yukarıdaki tabloda, varsayılan üyelik kullanıcısı şemasını alır ve ASP.NET Core kimliği şemaya eşler.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="bfdeb-170">Üyelik için kullanılan herhangi bir özel alanları el ile eşlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="bfdeb-171">Bu eşleme nebyla nalezena mapa Pro parolaları, parola ölçütlerini hem parola salts ikisi arasında geçirme gibi bulunur.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="bfdeb-172">**Parola null olarak bırakın ve kullanıcıların parolalarını sıfırlamalarına olanak istemeniz önerilir.**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="bfdeb-173">ASP.NET Core kimliği içinde `LockoutEnd` kullanıcıya kilitlenmişse bazı tarih gelecekte ayarlanması gerekir. Bu, geçiş öncesinde bir betik içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="bfdeb-174">Rolleri</span><span class="sxs-lookup"><span data-stu-id="bfdeb-174">Roles</span></span>

| <span data-ttu-id="bfdeb-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="bfdeb-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="bfdeb-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="bfdeb-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bfdeb-177">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-177">**Field Name**</span></span> | <span data-ttu-id="bfdeb-178">**Türü**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-178">**Type**</span></span>  |   <span data-ttu-id="bfdeb-179">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-179">**Field Name**</span></span> | <span data-ttu-id="bfdeb-180">**Türü**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="bfdeb-181">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-181">string</span></span> | `RoleId` | <span data-ttu-id="bfdeb-182">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-182">string</span></span>
|`Name` | <span data-ttu-id="bfdeb-183">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-183">string</span></span> | `RoleName` | <span data-ttu-id="bfdeb-184">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="bfdeb-185">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="bfdeb-186">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="bfdeb-187">Kullanıcı Rolleri</span><span class="sxs-lookup"><span data-stu-id="bfdeb-187">User Roles</span></span>

| <span data-ttu-id="bfdeb-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="bfdeb-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="bfdeb-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="bfdeb-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bfdeb-190">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-190">**Field Name**</span></span> | <span data-ttu-id="bfdeb-191">**Türü**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-191">**Type**</span></span>  |   <span data-ttu-id="bfdeb-192">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-192">**Field Name**</span></span> | <span data-ttu-id="bfdeb-193">**Türü**</span><span class="sxs-lookup"><span data-stu-id="bfdeb-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="bfdeb-194">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-194">string</span></span> | `RoleId` | <span data-ttu-id="bfdeb-195">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-195">string</span></span>
|`UserId` | <span data-ttu-id="bfdeb-196">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-196">string</span></span> | `UserId` | <span data-ttu-id="bfdeb-197">dize</span><span class="sxs-lookup"><span data-stu-id="bfdeb-197">string</span></span>

<span data-ttu-id="bfdeb-198">Önceki eşleme tabloları için geçiş betiği oluştururken başvuru *kullanıcılar* ve *rolleri*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="bfdeb-199">Aşağıdaki örnek, iki veritabanı bir veritabanı sunucusuna sahip olduğunuz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="bfdeb-200">Bir veritabanı mevcut ASP.NET üyelik şeması ve verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="bfdeb-201">Daha önce açıklanan adımları kullanarak diğer veritabanıyla oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="bfdeb-202">Daha fazla ayrıntı için eklenen satır içi açıklamalardır.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-202">Comments are included inline for more details.</span></span>

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
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

<span data-ttu-id="bfdeb-203">Bu betik tamamlandıktan sonra daha önce oluşturduğunuz ASP.NET Core kimliği uygulama üyelik kullanıcılarının ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="bfdeb-204">Kullanıcıların oturum açma önce parolalarını değiştirmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="bfdeb-205">Üyelik Sistemi kullanıcılara e-posta adresi ile eşleşmedi kullanıcı adları varsa bunu uygun hale getirmek için daha önce oluşturulan uygulama için değişiklik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="bfdeb-206">Varsayılan şablonu bekliyor `UserName` ve `Email` ile aynı.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="bfdeb-207">Bunlar farklı durumlar için oturum açma işlemi kullanmak için değiştirilmesi gerektiğinde `UserName` yerine `Email`.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="bfdeb-208">İçinde `PageModel` konumunda bulunan oturum açma sayfasının *Pages\Account\Login.cshtml.cs*, kaldırma `[EmailAddress]` özniteliğini *e-posta* özelliği.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="bfdeb-209">Yeniden adlandırın *UserName*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-209">Rename it to *UserName*.</span></span> <span data-ttu-id="bfdeb-210">Bu değişikliği gerektiren her yerde `EmailAddress` , içinde açıklanan *görünümü* ve *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="bfdeb-211">Sonuç aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="bfdeb-211">The result looks like the following:</span></span>

 ![Sabit oturum açma](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="bfdeb-213">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="bfdeb-213">Next steps</span></span>

<span data-ttu-id="bfdeb-214">Bu öğreticide, kullanıcıların SQL üyeliğinden ASP.NET Core 2.0 kimliği için bağlantı noktası öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="bfdeb-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="bfdeb-215">ASP.NET Core kimliği hakkında daha fazla bilgi için bkz. [kimliğe giriş](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="bfdeb-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
