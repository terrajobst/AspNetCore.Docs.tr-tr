---
title: "ASP.NET Çekirdeği'nde veri koruma"
author: rick-anderson
description: "Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: e08dea63f012c4a758f2e5561c4930d09cfee0ac
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="414f3-103">ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama</span><span class="sxs-lookup"><span data-stu-id="414f3-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="414f3-104">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="414f3-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="414f3-105">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="414f3-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="414f3-106">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="414f3-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="414f3-107">Tüketici API’lerine genel bakış</span><span class="sxs-lookup"><span data-stu-id="414f3-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="414f3-108">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="414f3-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="414f3-109">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="414f3-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="414f3-110">Parola karması</span><span class="sxs-lookup"><span data-stu-id="414f3-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="414f3-111">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="414f3-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="414f3-112">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="414f3-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="414f3-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="414f3-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="414f3-114">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="414f3-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="414f3-115">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="414f3-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="414f3-116">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="414f3-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="414f3-117">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="414f3-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="414f3-118">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="414f3-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="414f3-119">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="414f3-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="414f3-120">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="414f3-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="414f3-121">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="414f3-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="414f3-122">Uygulama</span><span class="sxs-lookup"><span data-stu-id="414f3-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="414f3-123">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="414f3-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="414f3-124">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="414f3-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="414f3-125">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="414f3-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="414f3-126">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="414f3-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="414f3-127">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="414f3-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="414f3-128">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="414f3-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="414f3-129">Anahtar değiştirilemezliği ve ayarları değiştirme</span><span class="sxs-lookup"><span data-stu-id="414f3-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="414f3-130">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="414f3-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="414f3-131">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="414f3-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="414f3-132">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="414f3-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="414f3-133">ASP.NET’te <machineKey>‘i değiştirme</span><span class="sxs-lookup"><span data-stu-id="414f3-133">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
