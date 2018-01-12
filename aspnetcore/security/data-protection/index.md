---
title: "ASP.NET Çekirdeği'nde veri koruma"
author: rick-anderson
description: "Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar."
keywords: ASP.NET Core, veri koruma
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="b599b-104">ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama</span><span class="sxs-lookup"><span data-stu-id="b599b-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="b599b-105">Veri koruma giriş</span><span class="sxs-lookup"><span data-stu-id="b599b-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="b599b-106">Veri Koruma API'ları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="b599b-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="b599b-107">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="b599b-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="b599b-108">Tüketici API'leri genel bakış</span><span class="sxs-lookup"><span data-stu-id="b599b-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="b599b-109">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="b599b-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="b599b-110">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="b599b-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="b599b-111">Parola karma</span><span class="sxs-lookup"><span data-stu-id="b599b-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="b599b-112">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="b599b-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="b599b-113">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="b599b-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="b599b-114">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b599b-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="b599b-115">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b599b-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="b599b-116">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="b599b-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="b599b-117">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="b599b-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="b599b-118">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="b599b-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="b599b-119">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="b599b-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="b599b-120">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="b599b-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="b599b-121">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="b599b-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="b599b-122">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="b599b-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="b599b-123">Uygulama</span><span class="sxs-lookup"><span data-stu-id="b599b-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="b599b-124">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="b599b-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="b599b-125">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="b599b-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="b599b-126">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="b599b-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="b599b-127">Anahtar Yönetimi</span><span class="sxs-lookup"><span data-stu-id="b599b-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="b599b-128">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b599b-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="b599b-129">REST anahtar şifrelemesi</span><span class="sxs-lookup"><span data-stu-id="b599b-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="b599b-130">Anahtar girişi ve ayarlarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="b599b-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="b599b-131">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="b599b-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="b599b-132">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="b599b-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="b599b-133">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="b599b-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="b599b-134">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="b599b-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="b599b-135">ASP.NET’te <machineKey>‘i değiştirme</span><span class="sxs-lookup"><span data-stu-id="b599b-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
