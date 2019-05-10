---
title: ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme
author: isaac2004
description: ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak varolan ASP.NET uygulamalarını geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65084870"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="55a1e-103">ASP.NET üyelik kimlik doğrulamasını ASP.NET Core 2.0 Identity'ye geçirme</span><span class="sxs-lookup"><span data-stu-id="55a1e-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="55a1e-104">Tarafından [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="55a1e-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="55a1e-105">Bu makalede, ASP.NET Core 2.0 kimliği için üyelik kimlik doğrulaması kullanarak ASP.NET uygulamaları için veritabanı şemasını geçirme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="55a1e-106">Bu belge, veritabanı şemasını ASP.NET üyelik tabanlı uygulamalar için ASP.NET Core kimliği için kullanılan veritabanı şemasını geçirmek için gerekli olan adımları sağlar.</span><span class="sxs-lookup"><span data-stu-id="55a1e-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="55a1e-107">ASP.NET üyelik tabanlı kimlik doğrulamasını ASP.NET Identity'ye geçirme hakkında daha fazla bilgi için bkz. [mevcut bir uygulamayı SQL üyeliğinden ASP.NET Identity'ye geçirme](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="55a1e-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="55a1e-108">ASP.NET Core kimliği hakkında daha fazla bilgi için bkz: [ASP.NET Core kimliği giriş](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="55a1e-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="55a1e-109">Üyelik şeması gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="55a1e-109">Review of Membership schema</span></span>

<span data-ttu-id="55a1e-110">ASP.NET 2.0 önce geliştiricilerin uygulamalarını tüm kimlik doğrulama ve yetkilendirme işlemi oluşturmaya görevli.</span><span class="sxs-lookup"><span data-stu-id="55a1e-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="55a1e-111">ASP.NET 2.0 ile ASP.NET uygulamaları içinde güvenlik işlemek için ortak bir çözüm sağlayarak üyelik sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="55a1e-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="55a1e-112">Geliştiriciler ile bir SQL Server veritabanına bir şema bootstrap için artık [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) komutu.</span><span class="sxs-lookup"><span data-stu-id="55a1e-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="55a1e-113">Bu komutu çalıştırdıktan sonra aşağıdaki tablolarda veritabanında oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="55a1e-113">After running this command, the following tables were created in the database.</span></span>

  ![Üyelik tablolarını](identity/_static/membership-tables.png)

<span data-ttu-id="55a1e-115">ASP.NET Core 2.0 kimliği için mevcut uygulamaları geçirmek için bu tablolardaki verilerin yeni kimlik şema tarafından kullanılan tablolar için geçirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="55a1e-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="55a1e-116">ASP.NET Core kimlik 2.0 şeması</span><span class="sxs-lookup"><span data-stu-id="55a1e-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="55a1e-117">ASP.NET Core 2.0 izleyen [kimlik](/aspnet/identity/index) ASP.NET 4.5 içinde tanıtılan ilkesi.</span><span class="sxs-lookup"><span data-stu-id="55a1e-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="55a1e-118">Uygulama çerçeveleri arasında ilkesini paylaşılan olsa bile ASP.NET Core sürümleri arasında farklı (bkz [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="55a1e-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="55a1e-119">ASP.NET Core 2.0 kimliği için şemasını görüntülemek için en hızlı yolu, yeni bir ASP.NET Core 2.0 uygulaması oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="55a1e-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="55a1e-120">Visual Studio 2017'de şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="55a1e-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="55a1e-121">**Dosya** > **Yeni** > **Proje**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="55a1e-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="55a1e-122">Yeni bir **ASP.NET Core Web uygulaması** adlı proje *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="55a1e-123">Seçin **ASP.NET Core 2.0** seçin ve açılan **Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="55a1e-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="55a1e-124">Bu şablon üreten bir [Razor sayfaları](xref:razor-pages/index) uygulama.</span><span class="sxs-lookup"><span data-stu-id="55a1e-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="55a1e-125">Tıklatmadan önce **Tamam**, tıklayın **kimlik doğrulamayı Değiştir**.</span><span class="sxs-lookup"><span data-stu-id="55a1e-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="55a1e-126">Seçin **bireysel kullanıcı hesapları** kimlik şablonları.</span><span class="sxs-lookup"><span data-stu-id="55a1e-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="55a1e-127">Son olarak, tıklayın **Tamam**, ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="55a1e-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="55a1e-128">Visual Studio, ASP.NET Core kimliği şablonunu kullanarak bir proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55a1e-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="55a1e-129">Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu** açmak için **PaketYöneticisiKonsolu** (PMC) penceresi.</span><span class="sxs-lookup"><span data-stu-id="55a1e-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="55a1e-130">Proje kök dizininde PMC gidin ve çalıştırma [Entity Framework (EF) çekirdek](/ef/core) `Update-Database` komutu.</span><span class="sxs-lookup"><span data-stu-id="55a1e-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="55a1e-131">ASP.NET Core 2.0 kimlik EF Core kimlik doğrulaması veri depolama veritabanı ile etkileşim kurmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="55a1e-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="55a1e-132">İçin yeni oluşturulan bir uygulamanın çalışması için sırayla var. Bu verileri depolamak için bir veritabanı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="55a1e-133">Yeni bir uygulama oluşturduktan sonra bir veritabanı ortam içinde şema incelemek için en hızlı yolu kullanarak veritabanını oluşturmak için olan [EF Core geçişleri](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="55a1e-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="55a1e-134">Bu işlem, bu şema taklit eden bir veritabanı, yerel olarak veya başka bir yerde, oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55a1e-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="55a1e-135">Daha fazla bilgi için yukarıdaki belgelerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="55a1e-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="55a1e-136">EF Core komutları belirtilen veritabanı için bağlantı dizesini kullanın *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="55a1e-137">Bir veritabanı bağlantı dizesi hedefleyen *localhost* adlı *asp net core kimliği*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="55a1e-138">Bu ayarda EF Core kullanacak şekilde yapılandırılmış `DefaultConnection` bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="55a1e-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="55a1e-139">Seçin **görünümü** > **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="55a1e-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="55a1e-140">Belirtilen veritabanı adı için karşılık gelen düğümünü `ConnectionStrings:DefaultConnection` özelliği *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="55a1e-141">`Update-Database` Komutu oluşturulan şemasıyla belirtilen veritabanı ve uygulama başlatma için gereken tüm verileri.</span><span class="sxs-lookup"><span data-stu-id="55a1e-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="55a1e-142">Aşağıdaki görüntüde ile önceki adımlarda oluşturulan tablo yapısı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Kimlik tabloları](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="55a1e-144">Geçiş şeması</span><span class="sxs-lookup"><span data-stu-id="55a1e-144">Migrate the schema</span></span>

<span data-ttu-id="55a1e-145">Tablo yapıları ve alanları üyelik hem de ASP.NET Core kimliği için küçük farklılıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="55a1e-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="55a1e-146">Desen, ASP.NET ve ASP.NET Core uygulamaları ile kimlik doğrulama/yetkilendirme için önemli ölçüde değişti.</span><span class="sxs-lookup"><span data-stu-id="55a1e-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="55a1e-147">Kimlikle hala kullanılan anahtar nesneler *kullanıcılar* ve *rolleri*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="55a1e-148">Eşleme tablolar için işte *kullanıcılar*, *rolleri*, ve *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="55a1e-149">Kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="55a1e-149">Users</span></span>

|<span data-ttu-id="55a1e-150">*Identity<br>(dbo.AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="55a1e-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="55a1e-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="55a1e-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="55a1e-152">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="55a1e-152">**Field Name**</span></span>                 |<span data-ttu-id="55a1e-153">**Tür**</span><span class="sxs-lookup"><span data-stu-id="55a1e-153">**Type**</span></span>|<span data-ttu-id="55a1e-154">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="55a1e-154">**Field Name**</span></span>                                    |<span data-ttu-id="55a1e-155">**Tür**</span><span class="sxs-lookup"><span data-stu-id="55a1e-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="55a1e-156">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="55a1e-157">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="55a1e-158">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="55a1e-159">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="55a1e-160">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="55a1e-161">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="55a1e-162">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="55a1e-163">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="55a1e-164">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="55a1e-165">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="55a1e-166">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="55a1e-167">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="55a1e-168">bit</span><span class="sxs-lookup"><span data-stu-id="55a1e-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="55a1e-169">bit</span><span class="sxs-lookup"><span data-stu-id="55a1e-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="55a1e-170">Tüm alan eşlemelerini üyeliğinin bire bir ilişkiler ASP.NET Core kimliği için benzer.</span><span class="sxs-lookup"><span data-stu-id="55a1e-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="55a1e-171">Yukarıdaki tabloda, varsayılan üyelik kullanıcısı şemasını alır ve ASP.NET Core kimliği şemaya eşler.</span><span class="sxs-lookup"><span data-stu-id="55a1e-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="55a1e-172">Üyelik için kullanılan herhangi bir özel alanları el ile eşlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="55a1e-173">Bu eşleme nebyla nalezena mapa Pro parolaları, parola ölçütlerini hem parola salts ikisi arasında geçirme gibi bulunur.</span><span class="sxs-lookup"><span data-stu-id="55a1e-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="55a1e-174">**Parola null olarak bırakın ve kullanıcıların parolalarını sıfırlamalarına olanak istemeniz önerilir.**</span><span class="sxs-lookup"><span data-stu-id="55a1e-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="55a1e-175">ASP.NET Core kimliği içinde `LockoutEnd` kullanıcıya kilitlenmişse bazı tarih gelecekte ayarlanması gerekir. Bu, geçiş öncesinde bir betik içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="55a1e-176">Rolleri</span><span class="sxs-lookup"><span data-stu-id="55a1e-176">Roles</span></span>

|<span data-ttu-id="55a1e-177">*Kimlik<br>(dbo. AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="55a1e-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="55a1e-178">*Üyelik<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="55a1e-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="55a1e-179">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="55a1e-179">**Field Name**</span></span>                 |<span data-ttu-id="55a1e-180">**Tür**</span><span class="sxs-lookup"><span data-stu-id="55a1e-180">**Type**</span></span>|<span data-ttu-id="55a1e-181">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="55a1e-181">**Field Name**</span></span>   |<span data-ttu-id="55a1e-182">**Tür**</span><span class="sxs-lookup"><span data-stu-id="55a1e-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="55a1e-183">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-183">string</span></span>  |`RoleId`         | <span data-ttu-id="55a1e-184">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="55a1e-185">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-185">string</span></span>  |`RoleName`       | <span data-ttu-id="55a1e-186">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="55a1e-187">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="55a1e-188">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="55a1e-189">Kullanıcı Rolleri</span><span class="sxs-lookup"><span data-stu-id="55a1e-189">User Roles</span></span>

|<span data-ttu-id="55a1e-190">*Identity<br>(dbo.AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="55a1e-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="55a1e-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="55a1e-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="55a1e-192">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="55a1e-192">**Field Name**</span></span>           |<span data-ttu-id="55a1e-193">**Tür**</span><span class="sxs-lookup"><span data-stu-id="55a1e-193">**Type**</span></span>  |<span data-ttu-id="55a1e-194">**Alan adı**</span><span class="sxs-lookup"><span data-stu-id="55a1e-194">**Field Name**</span></span>|<span data-ttu-id="55a1e-195">**Tür**</span><span class="sxs-lookup"><span data-stu-id="55a1e-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="55a1e-196">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-196">string</span></span>    |`RoleId`      |<span data-ttu-id="55a1e-197">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="55a1e-198">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-198">string</span></span>    |`UserId`      |<span data-ttu-id="55a1e-199">dize</span><span class="sxs-lookup"><span data-stu-id="55a1e-199">string</span></span>                     |

<span data-ttu-id="55a1e-200">Önceki eşleme tabloları için geçiş betiği oluştururken başvuru *kullanıcılar* ve *rolleri*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="55a1e-201">Aşağıdaki örnek, iki veritabanı bir veritabanı sunucusuna sahip olduğunuz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="55a1e-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="55a1e-202">Bir veritabanı mevcut ASP.NET üyelik şeması ve verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="55a1e-203">Diğer *CoreIdentitySample* daha önce açıklanan adımları kullanarak veritabanı oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="55a1e-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="55a1e-204">Daha fazla ayrıntı için eklenen satır içi açıklamalardır.</span><span class="sxs-lookup"><span data-stu-id="55a1e-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="55a1e-205">Önceki komut tamamlandıktan sonra daha önce oluşturduğunuz ASP.NET Core kimliği uygulama üyelik kullanıcılarının ile doldurulur.</span><span class="sxs-lookup"><span data-stu-id="55a1e-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="55a1e-206">Kullanıcıların oturum açma önce parolalarını değiştirmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="55a1e-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="55a1e-207">Üyelik Sistemi kullanıcılara e-posta adresi ile eşleşmedi kullanıcı adları varsa bunu uygun hale getirmek için daha önce oluşturulan uygulama için değişiklik gerekmez.</span><span class="sxs-lookup"><span data-stu-id="55a1e-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="55a1e-208">Varsayılan şablonu bekliyor `UserName` ve `Email` ile aynı.</span><span class="sxs-lookup"><span data-stu-id="55a1e-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="55a1e-209">Bunlar farklı durumlar için oturum açma işlemi kullanmak için değiştirilmesi gerektiğinde `UserName` yerine `Email`.</span><span class="sxs-lookup"><span data-stu-id="55a1e-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="55a1e-210">İçinde `PageModel` konumunda bulunan oturum açma sayfasının *Pages\Account\Login.cshtml.cs*, kaldırma `[EmailAddress]` özniteliğini *e-posta* özelliği.</span><span class="sxs-lookup"><span data-stu-id="55a1e-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="55a1e-211">Yeniden adlandırın *UserName*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-211">Rename it to *UserName*.</span></span> <span data-ttu-id="55a1e-212">Bu değişikliği gerektiren her yerde `EmailAddress` , içinde açıklanan *görünümü* ve *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="55a1e-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="55a1e-213">Sonuç aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="55a1e-213">The result looks like the following:</span></span>

 ![Sabit oturum açma](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="55a1e-215">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="55a1e-215">Next steps</span></span>

<span data-ttu-id="55a1e-216">Bu öğreticide, kullanıcıların SQL üyeliğinden ASP.NET Core 2.0 kimliği için bağlantı noktası öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="55a1e-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="55a1e-217">ASP.NET Core kimliği hakkında daha fazla bilgi için bkz. [kimliğe giriş](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="55a1e-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
