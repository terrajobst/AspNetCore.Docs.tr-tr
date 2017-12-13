---
uid: web-api/overview/security/integrated-windows-authentication
title: "Tümleşik Windows kimlik doğrulaması | Microsoft Docs"
author: MikeWasson
description: "ASP.NET Web API'de tümleşik Windows kimlik doğrulaması kullanmayı açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="134f9-103">Tümleşik Windows kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="134f9-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="134f9-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="134f9-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="134f9-105">Tümleşik Windows kimlik doğrulaması, Kerberos veya NTLM kullanarak kendi Windows kimlik bilgileriyle oturum açmalarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="134f9-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="134f9-106">İstemci kimlik bilgileri yetkilendirme üst bilgi gönderir.</span><span class="sxs-lookup"><span data-stu-id="134f9-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="134f9-107">Windows kimlik doğrulaması intranet ortamı için idealdir.</span><span class="sxs-lookup"><span data-stu-id="134f9-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="134f9-108">Daha fazla bilgi için bkz: [Windows kimlik doğrulaması](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="134f9-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="134f9-109">Yararları</span><span class="sxs-lookup"><span data-stu-id="134f9-109">Advantages</span></span> | <span data-ttu-id="134f9-110">Dezavantajlar</span><span class="sxs-lookup"><span data-stu-id="134f9-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="134f9-111">-IIS ile oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="134f9-111">- Built into IIS.</span></span> <span data-ttu-id="134f9-112">-Kullanıcı kimlik bilgilerinin istekte göndermez.</span><span class="sxs-lookup"><span data-stu-id="134f9-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="134f9-113">-İstemci bilgisayar (örneğin, intranet uygulama) etki alanına aitse, kullanıcı kimlik bilgilerini girmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="134f9-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="134f9-114">-Internet uygulamaları için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="134f9-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="134f9-115">-İstemci Kerberos veya NTLM desteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="134f9-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="134f9-116">-İstemci Active Directory etki alanında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="134f9-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="134f9-117">Uygulamanızı Azure üzerinde barındırılan ve bir şirket içi Active Directory etki alanı varsa, şirket içi AD Azure Active Directory ile Federasyon göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="134f9-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="134f9-118">Böylece, kullanıcılar kendi şirket içi kimlik bilgileriyle oturum açabilir, ancak kimlik doğrulaması Azure AD tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="134f9-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="134f9-119">Daha fazla bilgi için bkz: [Azure kimlik doğrulaması](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="134f9-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="134f9-120">Tümleşik Windows kimlik doğrulaması kullanan bir uygulama oluşturmak için MVC 4 Proje Sihirbazı'nda "Intranet uygulaması" şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="134f9-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="134f9-121">Bu proje şablonu Web.config dosyasında aşağıdaki ayar yapar:</span><span class="sxs-lookup"><span data-stu-id="134f9-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="134f9-122">İstemci tarafında, tümleşik Windows kimlik doğrulamasını destekleyen bir tarayıcı ile çalışır [anlaş](http://www.ietf.org/rfc/rfc4559.txt) en önde gelen tarayıcılar içeren kimlik doğrulama düzeni.</span><span class="sxs-lookup"><span data-stu-id="134f9-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="134f9-123">.NET istemci uygulamaları için **HttpClient** sınıfı, Windows kimlik doğrulamasını destekler:</span><span class="sxs-lookup"><span data-stu-id="134f9-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="134f9-124">Windows kimlik doğrulaması için siteler arası istek sahteciliği (CSRF) saldırılarına karşı savunmasızdır.</span><span class="sxs-lookup"><span data-stu-id="134f9-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="134f9-125">Bkz: [siteler arası istek sahtekarlığı (CSRF) saldırılarını önleme](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="134f9-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
