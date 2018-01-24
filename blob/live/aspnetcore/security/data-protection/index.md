---
title: "ASP.NET Çekirdeği'nde veri koruma"
author: rick-anderson
description: "Bu belge, çeşitli ASP.NET Core veri koruma konular için içindekiler tablosu olarak görev yapar."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 151385964d877fc9eadaa219320e5f5a195164e4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="649cc-103">ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama</span><span class="sxs-lookup"><span data-stu-id="649cc-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="649cc-104">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="649cc-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="649cc-105">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="649cc-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="649cc-106">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="649cc-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="649cc-107">Tüketici API’lerine genel bakış</span><span class="sxs-lookup"><span data-stu-id="649cc-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="649cc-108">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="649cc-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="649cc-109">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="649cc-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="649cc-110">Parola karması</span><span class="sxs-lookup"><span data-stu-id="649cc-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="649cc-111">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="649cc-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="649cc-112">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="649cc-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="649cc-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="649cc-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="649cc-114">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="649cc-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="649cc-115">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="649cc-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="649cc-116">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="649cc-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="649cc-117">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="649cc-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="649cc-118">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="649cc-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="649cc-119">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="649cc-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="649cc-120">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="649cc-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="649cc-121">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="649cc-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="649cc-122">Uygulama</span><span class="sxs-lookup"><span data-stu-id="649cc-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="649cc-123">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="649cc-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="649cc-124">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="649cc-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="649cc-125">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="649cc-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="649cc-126">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="649cc-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="649cc-127">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="649cc-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="649cc-128">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="649cc-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="649cc-129">Anahtar değiştirilemezliği ve ayarları değiştirme</span><span class="sxs-lookup"><span data-stu-id="649cc-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="649cc-130">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="649cc-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="649cc-131">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="649cc-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="649cc-132">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="649cc-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="649cc-133">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="649cc-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="649cc-134">ASP.NET’te <machineKey>‘i değiştirme</span><span class="sxs-lookup"><span data-stu-id="649cc-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
