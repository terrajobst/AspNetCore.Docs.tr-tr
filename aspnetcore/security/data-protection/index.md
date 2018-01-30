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
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="7b1f7-103">ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama</span><span class="sxs-lookup"><span data-stu-id="7b1f7-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="7b1f7-104">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="7b1f7-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="7b1f7-105">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="7b1f7-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="7b1f7-106">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="7b1f7-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="7b1f7-107">Tüketici API’lerine genel bakış</span><span class="sxs-lookup"><span data-stu-id="7b1f7-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="7b1f7-108">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="7b1f7-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="7b1f7-109">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="7b1f7-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="7b1f7-110">Parola karması</span><span class="sxs-lookup"><span data-stu-id="7b1f7-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="7b1f7-111">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="7b1f7-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="7b1f7-112">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="7b1f7-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="7b1f7-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7b1f7-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="7b1f7-114">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="7b1f7-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="7b1f7-115">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="7b1f7-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="7b1f7-116">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="7b1f7-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="7b1f7-117">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="7b1f7-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="7b1f7-118">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="7b1f7-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="7b1f7-119">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="7b1f7-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="7b1f7-120">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="7b1f7-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="7b1f7-121">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="7b1f7-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="7b1f7-122">Uygulama</span><span class="sxs-lookup"><span data-stu-id="7b1f7-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="7b1f7-123">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="7b1f7-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="7b1f7-124">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="7b1f7-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="7b1f7-125">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="7b1f7-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="7b1f7-126">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="7b1f7-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="7b1f7-127">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="7b1f7-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="7b1f7-128">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="7b1f7-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="7b1f7-129">Anahtar değiştirilemezliği ve ayarları değiştirme</span><span class="sxs-lookup"><span data-stu-id="7b1f7-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="7b1f7-130">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="7b1f7-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="7b1f7-131">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="7b1f7-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="7b1f7-132">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="7b1f7-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="7b1f7-133">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="7b1f7-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="7b1f7-134">ASP.NET’te <machineKey>‘i değiştirme</span><span class="sxs-lookup"><span data-stu-id="7b1f7-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
