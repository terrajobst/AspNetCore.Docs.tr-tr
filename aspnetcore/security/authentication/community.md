---
title: ASP.NET Core için topluluk OSS kimlik doğrulama seçenekleri
author: rick-anderson
description: ASP.NET Core için açık kaynak kimlik doğrulama seçenekleri keşfedin.
ms.author: riande
ms.date: 02/15/2019
uid: security/authentication/community
ms.openlocfilehash: e25df794bdff8f904382e7a299755ae4c23b892e
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410290"
---
# <a name="community-oss-authentication-options-for-aspnet-core"></a><span data-ttu-id="46156-103">ASP.NET Core için topluluk OSS kimlik doğrulama seçenekleri</span><span class="sxs-lookup"><span data-stu-id="46156-103">Community OSS authentication options for ASP.NET Core</span></span>

<span data-ttu-id="46156-104">Bu sayfa, ASP.NET Core için topluluk tarafından sağlanan, açık kaynaklı kimlik doğrulama seçenekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="46156-104">This page contains community-provided, open source authentication options for ASP.NET Core.</span></span> <span data-ttu-id="46156-105">Bu sayfa, kullanılabilir yeni sağlayıcıları olarak düzenli olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="46156-105">This page is periodically updated as new providers become available.</span></span>

## <a name="oss-authentication-providers"></a><span data-ttu-id="46156-106">OSS kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="46156-106">OSS authentication providers</span></span>

<span data-ttu-id="46156-107">Aşağıdaki liste, alfabetik olarak sıralanır.</span><span class="sxs-lookup"><span data-stu-id="46156-107">The list below is sorted alphabetically.</span></span>

| <span data-ttu-id="46156-108">Ad</span><span class="sxs-lookup"><span data-stu-id="46156-108">Name</span></span> | <span data-ttu-id="46156-109">Açıklama</span><span class="sxs-lookup"><span data-stu-id="46156-109">Description</span></span> |
| ---- | ----------- |
| [<span data-ttu-id="46156-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span><span class="sxs-lookup"><span data-stu-id="46156-110">AspNet.Security.OpenIdConnect.Server (ASOS)</span></span>](https://github.com/aspnet-contrib/AspNet.Security.OpenIdConnect.Server) | <span data-ttu-id="46156-111">ASOS bir alt düzey ve protokolü öncelikli Openıd Connect sunucusu için ASP.NET Core ve OWIN/Katana çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="46156-111">ASOS is a low-level, protocol-first OpenID Connect server framework for ASP.NET Core and OWIN/Katana.</span></span> |
| [<span data-ttu-id="46156-112">Cierge</span><span class="sxs-lookup"><span data-stu-id="46156-112">Cierge</span></span>](https://github.com/pwdless/Cierge) | <span data-ttu-id="46156-113">Cierge kullanıcı kaydolma, oturum açma, profiller, yönetim ve sosyal oturumların işleyen bir Openıd Connect sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="46156-113">Cierge is an OpenID Connect server that handles user signup, login, profiles, management, and social logins.</span></span> |
| [<span data-ttu-id="46156-114">Gluu sunucusu</span><span class="sxs-lookup"><span data-stu-id="46156-114">Gluu Server</span></span>](https://gluu.org/) | <span data-ttu-id="46156-115">Kurumsal kullanıma hazır, açık kaynak yazılım için kimlik, erişim yönetimi (IAM) ve çoklu oturum açma (SSO).</span><span class="sxs-lookup"><span data-stu-id="46156-115">Enterprise ready, open source software for identity, access management (IAM), and single sign-on (SSO).</span></span> <span data-ttu-id="46156-116">Daha fazla bilgi için [Gluu ürün belgelerine](https://gluu.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="46156-116">For more information, see the [Gluu Product Documentation](https://gluu.org/docs/).</span></span> |
| [<span data-ttu-id="46156-117">IdentityServer</span><span class="sxs-lookup"><span data-stu-id="46156-117">IdentityServer</span></span>](https://identityserver.io/) | <span data-ttu-id="46156-118">IdentityServer idare .NET Foundation'ın altında ve Openıd Foundation tarafından resmi olarak sertifikalı, ASP.NET Core için Openıd Connect ve OAuth 2.0 bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="46156-118">IdentityServer is an OpenID Connect and OAuth 2.0 framework for ASP.NET Core, officially certified by the OpenID Foundation and under governance of the .NET Foundation.</span></span> <span data-ttu-id="46156-119">Daha fazla bilgi için [Hoş Geldiniz Identityserver4 (belgeler)](https://identityserver4.readthedocs.io/en/latest/).</span><span class="sxs-lookup"><span data-stu-id="46156-119">For more information, see [Welcome to IdentityServer4 (Documentation)](https://identityserver4.readthedocs.io/en/latest/).</span></span> |
| [<span data-ttu-id="46156-120">OpenIddict</span><span class="sxs-lookup"><span data-stu-id="46156-120">OpenIddict</span></span>](https://github.com/openiddict/openiddict-core) | <span data-ttu-id="46156-121">OpenIddict kullanımı kolay Openıd Connect yönelik bir ASP.NET Core sunucusudur.</span><span class="sxs-lookup"><span data-stu-id="46156-121">OpenIddict is an easy-to-use OpenID Connect server for ASP.NET Core.</span></span> |

<span data-ttu-id="46156-122">Bir sağlayıcı eklemek için [bu sayfayı düzenleme](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span><span class="sxs-lookup"><span data-stu-id="46156-122">To add a provider, [edit this page](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Faspnet%2FDocs%2Fedit%2Fmaster%2Faspnetcore%2Fsecurity%2Fauthentication%2Fcommunity.md).</span></span>
