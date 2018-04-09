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
ms.openlocfilehash: 7ba1966dcaf1ce0510f489cfe0ff11501faffa56
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="83f05-103">ASP.NET Core yetkilendirme giriş</span><span class="sxs-lookup"><span data-stu-id="83f05-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="83f05-104">Yetkilendirme başvuruyor ne belirleyen işlem için bir kullanıcı yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="83f05-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="83f05-105">Örneğin, belge kitaplığı oluşturmak, belge ekleme, belgeleri düzenlemek ve bunları silmek için bir yönetici kullanıcı izin.</span><span class="sxs-lookup"><span data-stu-id="83f05-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="83f05-106">Kitaplıkla çalışırken yönetici olmayan bir kullanıcı yalnızca belgeleri okuma yetkisi.</span><span class="sxs-lookup"><span data-stu-id="83f05-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="83f05-107">Yetkilendirme resme ve bağımsız olan bir kullanıcı ascertaining işlemidir kimlik doğrulaması gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="83f05-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="83f05-108">Kimlik doğrulama geçerli kullanıcı için bir veya daha fazla kimlikleri oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="83f05-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="83f05-109">Yetkilendirme türleri</span><span class="sxs-lookup"><span data-stu-id="83f05-109">Authorization types</span></span>

<span data-ttu-id="83f05-110">ASP.NET Core yetkilendirme sağlayan basit, bildirim temelli [rol](xref:security/authorization/roles) ve zengin bir [ilke tabanlı](xref:security/authorization/policies) modeli.</span><span class="sxs-lookup"><span data-stu-id="83f05-110">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="83f05-111">Yetkilendirme gereksinimleri ifade edilir ve gereksinimleri karşı bir kullanıcının talebi işleyicileri değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="83f05-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="83f05-112">Kesinlik temelli denetimleri basit ve kullanıcı kimliğine ve kullanıcının erişmeye çalıştığı kaynak özelliklerini değerlendirmek ilkelerindeki üzerinde temel alabilir.</span><span class="sxs-lookup"><span data-stu-id="83f05-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="83f05-113">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="83f05-113">Namespaces</span></span>

<span data-ttu-id="83f05-114">Yetkilendirme bileşenleri de dahil olmak üzere, `AuthorizeAttribute` ve `AllowAnonymousAttribute` içinde bulunan öznitelikler `Microsoft.AspNetCore.Authorization` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="83f05-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="83f05-115">Üzerinde belgelere [basit yetkilendirme](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="83f05-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
