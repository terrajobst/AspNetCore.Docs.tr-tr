---
title: ASP.NET Core yetkilendirme için giriş
author: rick-anderson
description: ASP.NET Core uygulamalarda yetkilendirme ve yetkilendirme ile ilgili temel bilgileri öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660715"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="4a8d0-103">ASP.NET Core yetkilendirme için giriş</span><span class="sxs-lookup"><span data-stu-id="4a8d0-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="4a8d0-104">Yetkilendirme, bir kullanıcının ne yapabileceğini belirleyen işlemi ifade eder.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="4a8d0-105">Örneğin, bir yönetici kullanıcının belge kitaplığı oluşturmalarına, belge eklemesine, belge düzenlemesine ve bunları silmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="4a8d0-106">Kitaplıkla çalışan yönetici olmayan bir Kullanıcı yalnızca belgeleri okuma yetkisine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="4a8d0-107">Yetkilendirme, kimlik doğrulamasından dik ve bağımsızdır.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="4a8d0-108">Ancak, yetkilendirme bir kimlik doğrulama mekanizması gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="4a8d0-109">Kimlik doğrulaması, kullanıcı kim olduğunu belirlememe işlemidir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="4a8d0-110">Kimlik doğrulaması geçerli kullanıcı için bir veya daha fazla kimlik oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-110">Authentication may create one or more identities for the current user.</span></span>

<span data-ttu-id="4a8d0-111">ASP.NET Core kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/index>.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-111">For more information about authentication in ASP.NET Core, see <xref:security/authentication/index>.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="4a8d0-112">Yetkilendirme türleri</span><span class="sxs-lookup"><span data-stu-id="4a8d0-112">Authorization types</span></span>

<span data-ttu-id="4a8d0-113">ASP.NET Core yetkilendirme basit, bildirime dayalı bir [rol](xref:security/authorization/roles) ve zengin bir [ilke tabanlı](xref:security/authorization/policies) model sağlar.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-113">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="4a8d0-114">Yetkilendirme, gereksinimlerde ifade edilir ve işleyiciler bir kullanıcının gereksinimlerine göre taleplerini değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-114">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="4a8d0-115">Zorunlu denetimler, kullanıcının erişmeye çalıştığı kaynağın Kullanıcı kimliğini ve özelliklerini değerlendiren basit ilkeleri veya ilkeleri temel alabilir.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-115">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="4a8d0-116">Ad Alanları</span><span class="sxs-lookup"><span data-stu-id="4a8d0-116">Namespaces</span></span>

<span data-ttu-id="4a8d0-117">`AuthorizeAttribute` ve `AllowAnonymousAttribute` öznitelikleri dahil olmak üzere yetkilendirme bileşenleri `Microsoft.AspNetCore.Authorization` ad alanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-117">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="4a8d0-118">[Basit yetkilendirme](xref:security/authorization/simple)ile ilgili belgelere başvurun.</span><span class="sxs-lookup"><span data-stu-id="4a8d0-118">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
