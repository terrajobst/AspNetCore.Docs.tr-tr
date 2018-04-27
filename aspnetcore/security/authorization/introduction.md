---
title: ASP.NET Core yetkilendirme giriş
author: rick-anderson
description: Yetkilendirme ve ASP.NET Core uygulamaları yetkilendirme nasıl çalıştığını temel bilgileri öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="8bd57-103">ASP.NET Core yetkilendirme giriş</span><span class="sxs-lookup"><span data-stu-id="8bd57-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="8bd57-104">Yetkilendirme başvuruyor ne belirleyen işlem için bir kullanıcı yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bd57-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="8bd57-105">Örneğin, belge kitaplığı oluşturmak, belge ekleme, belgeleri düzenlemek ve bunları silmek için bir yönetici kullanıcı izin.</span><span class="sxs-lookup"><span data-stu-id="8bd57-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="8bd57-106">Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okuma yetkisi.</span><span class="sxs-lookup"><span data-stu-id="8bd57-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="8bd57-107">Yetkilendirme resme ve kimlik doğrulama bağımsız gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="8bd57-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="8bd57-108">Ancak, yetkilendirme kimlik doğrulama mekanizması gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8bd57-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="8bd57-109">Kimlik doğrulaması kullanan bir kullanıcı ascertaining işlemidir.</span><span class="sxs-lookup"><span data-stu-id="8bd57-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="8bd57-110">Kimlik doğrulama geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="8bd57-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="8bd57-111">Yetkilendirme türleri</span><span class="sxs-lookup"><span data-stu-id="8bd57-111">Authorization types</span></span>

<span data-ttu-id="8bd57-112">ASP.NET Core yetkilendirme sağlayan basit, bildirim temelli [rol](xref:security/authorization/roles) ve zengin bir [ilke tabanlı](xref:security/authorization/policies) modeli.</span><span class="sxs-lookup"><span data-stu-id="8bd57-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="8bd57-113">Yetkilendirme gereksinimleri ifade edilir ve gereksinimleri karşı bir kullanıcının talebi işleyicileri değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="8bd57-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="8bd57-114">Kesinlik temelli denetimleri basit ve kullanıcı kimliğine ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkelerindeki üzerinde temel alabilir.</span><span class="sxs-lookup"><span data-stu-id="8bd57-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="8bd57-115">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="8bd57-115">Namespaces</span></span>

<span data-ttu-id="8bd57-116">Yetkilendirme bileşenleri de dahil olmak üzere, `AuthorizeAttribute` ve `AllowAnonymousAttribute` içinde bulunan öznitelikler `Microsoft.AspNetCore.Authorization` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="8bd57-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="8bd57-117">Üzerinde belgelere [basit yetkilendirme](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="8bd57-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
