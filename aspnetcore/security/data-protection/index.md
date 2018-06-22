---
title: ASP.NET Çekirdeği'nde veri koruma
author: rick-anderson
description: Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277130"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="ae767-103">ASP.NET Çekirdeği'nde veri koruma</span><span class="sxs-lookup"><span data-stu-id="ae767-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="ae767-104">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="ae767-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="ae767-105">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ae767-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="ae767-106">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="ae767-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="ae767-107">Tüketici API’lerine genel bakış</span><span class="sxs-lookup"><span data-stu-id="ae767-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="ae767-108">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="ae767-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="ae767-109">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="ae767-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="ae767-110">Karma parolalar</span><span class="sxs-lookup"><span data-stu-id="ae767-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="ae767-111">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="ae767-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="ae767-112">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="ae767-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="ae767-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae767-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="ae767-114">ASP.NET Core veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae767-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="ae767-115">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="ae767-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="ae767-116">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="ae767-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="ae767-117">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="ae767-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="ae767-118">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="ae767-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="ae767-119">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="ae767-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="ae767-120">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="ae767-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="ae767-121">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="ae767-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="ae767-122">Uygulama</span><span class="sxs-lookup"><span data-stu-id="ae767-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="ae767-123">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="ae767-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="ae767-124">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="ae767-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="ae767-125">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="ae767-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="ae767-126">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="ae767-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="ae767-127">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="ae767-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="ae767-128">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="ae767-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="ae767-129">Anahtar değiştirilemezliği ve ayarlar</span><span class="sxs-lookup"><span data-stu-id="ae767-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="ae767-130">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="ae767-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="ae767-131">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="ae767-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="ae767-132">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="ae767-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="ae767-133">ASP.NET değiştirme <machineKey> ASP.NET Core içinde</span><span class="sxs-lookup"><span data-stu-id="ae767-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
