---
title: ASP.NET Core yetkilendirme giriş
author: rick-anderson
description: Yetkilendirme ve yetkilendirme ASP.NET Core uygulamalarında nasıl çalıştığı hakkındaki temel bilgileri öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902846"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="de13c-103">ASP.NET Core yetkilendirme giriş</span><span class="sxs-lookup"><span data-stu-id="de13c-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="de13c-104">Yetkilendirme ne belirleyen işleme başvuran bir kullanıcı işlemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="de13c-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="de13c-105">Örneğin, bir belge kitaplığı oluşturma, belgelere ekleme, belgeleri düzenlemeye ve silmek için bir yönetici kullanıcı izin.</span><span class="sxs-lookup"><span data-stu-id="de13c-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="de13c-106">Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okumak için yetkili.</span><span class="sxs-lookup"><span data-stu-id="de13c-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="de13c-107">Yetkilendirme, dikey ve kimlik doğrulaması'dan bağımsız gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="de13c-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="de13c-108">Ancak, bir kimlik doğrulama mekanizması yetkilendirme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="de13c-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="de13c-109">Kimlik doğrulaması kullanan bir kullanıcıdır ascertaining işlemidir.</span><span class="sxs-lookup"><span data-stu-id="de13c-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="de13c-110">Kimlik doğrulaması, geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="de13c-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="de13c-111">Yetkilendirme türleri</span><span class="sxs-lookup"><span data-stu-id="de13c-111">Authorization types</span></span>

<span data-ttu-id="de13c-112">ASP.NET Core yetkilendirme sağlayan basit, bildirim temelli [rol](xref:security/authorization/roles) ve zengin [ilke tabanlı](xref:security/authorization/policies) modeli.</span><span class="sxs-lookup"><span data-stu-id="de13c-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="de13c-113">Yetkilendirme gereksinimleri ifade edilir ve işleyicileri bir kullanıcının talebi gereksinimleriyle karşılaştırarak değerlendirmek.</span><span class="sxs-lookup"><span data-stu-id="de13c-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="de13c-114">Kesinlik temelli denetimleri basit ilkeleri veya kullanıcı kimliği ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkeleri temel alabilir.</span><span class="sxs-lookup"><span data-stu-id="de13c-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="de13c-115">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="de13c-115">Namespaces</span></span>

<span data-ttu-id="de13c-116">Yetkilendirme bileşenlerine dahil `AuthorizeAttribute` ve `AllowAnonymousAttribute` öznitelikleri içinde bulunan `Microsoft.AspNetCore.Authorization` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="de13c-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="de13c-117">Üzerinde belgelerine [basit yetkilendirme](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="de13c-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
