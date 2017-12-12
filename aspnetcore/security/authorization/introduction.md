---
title: "Yetkilendirme giriş"
author: rick-anderson
description: "Bu belge yetkilendirme temel bir açıklamasını sağlar ve ASP.NET Core yetkilendirme nasıl ilişkili olduğu açıklanmaktadır."
keywords: ASP.NET Core, yetkilendirme
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a><span data-ttu-id="67d28-104">Giriş</span><span class="sxs-lookup"><span data-stu-id="67d28-104">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="67d28-105">Yetkilendirme başvuruyor ne belirleyen işlem için bir kullanıcı yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="67d28-105">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="67d28-106">Örneğin, belge kitaplığı oluşturmak, belge ekleme, belgeleri düzenlemek ve bunları silmek için bir yönetici kullanıcı izin.</span><span class="sxs-lookup"><span data-stu-id="67d28-106">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="67d28-107">Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okuma yetkisi.</span><span class="sxs-lookup"><span data-stu-id="67d28-107">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="67d28-108">Yetkilendirme resme ve bağımsız olan bir kullanıcı ascertaining işlemidir kimlik doğrulaması gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="67d28-108">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="67d28-109">Kimlik doğrulama geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="67d28-109">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="67d28-110">Yetkilendirme türleri</span><span class="sxs-lookup"><span data-stu-id="67d28-110">Authorization Types</span></span>

<span data-ttu-id="67d28-111">ASP.NET Core yetkilendirme sağlayan basit bir bildirim temelli [rol](roles.md) ve [zengin ilke tabanlı](policies.md) modeli.</span><span class="sxs-lookup"><span data-stu-id="67d28-111">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="67d28-112">Yetkilendirme gereksinimleri ifade edilir ve gereksinimleri karşı bir kullanıcının talebi işleyicileri değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="67d28-112">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="67d28-113">Kesinlik temelli denetimleri basit ve kullanıcı kimliğine ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkelerindeki üzerinde temel alabilir.</span><span class="sxs-lookup"><span data-stu-id="67d28-113">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="67d28-114">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="67d28-114">Namespaces</span></span>

<span data-ttu-id="67d28-115">Yetkilendirme bileşenleri de dahil olmak üzere, `AuthorizeAttribute` ve `AllowAnonymousAttribute` içinde bulunan öznitelikler `Microsoft.AspNetCore.Authorization` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="67d28-115">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
