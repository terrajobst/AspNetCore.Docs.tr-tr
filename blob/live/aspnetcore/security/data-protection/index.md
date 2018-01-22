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
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="2bd25-103">ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama</span><span class="sxs-lookup"><span data-stu-id="2bd25-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="2bd25-104">Veri korumaya giriş</span><span class="sxs-lookup"><span data-stu-id="2bd25-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="2bd25-105">Veri Koruma API’lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="2bd25-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="2bd25-106">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="2bd25-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="2bd25-107">Tüketici API’lerine genel bakış</span><span class="sxs-lookup"><span data-stu-id="2bd25-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="2bd25-108">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="2bd25-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="2bd25-109">Amaç hiyerarşisi ve çok kiracılılık</span><span class="sxs-lookup"><span data-stu-id="2bd25-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="2bd25-110">Parola karması</span><span class="sxs-lookup"><span data-stu-id="2bd25-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="2bd25-111">Korumalı yüklerin ömrünü sınırlama</span><span class="sxs-lookup"><span data-stu-id="2bd25-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="2bd25-112">Anahtarları iptal edilen yüklerin korumasını kaldırma</span><span class="sxs-lookup"><span data-stu-id="2bd25-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="2bd25-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2bd25-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="2bd25-114">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2bd25-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="2bd25-115">Varsayılan ayarlar</span><span class="sxs-lookup"><span data-stu-id="2bd25-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="2bd25-116">Makine geneli ilke</span><span class="sxs-lookup"><span data-stu-id="2bd25-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="2bd25-117">DI kullanan olmayan senaryolar</span><span class="sxs-lookup"><span data-stu-id="2bd25-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="2bd25-118">Genişletilebilirlik API’leri</span><span class="sxs-lookup"><span data-stu-id="2bd25-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="2bd25-119">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="2bd25-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="2bd25-120">Anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="2bd25-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="2bd25-121">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="2bd25-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="2bd25-122">Uygulama</span><span class="sxs-lookup"><span data-stu-id="2bd25-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="2bd25-123">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="2bd25-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="2bd25-124">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="2bd25-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="2bd25-125">Bağlam üst bilgileri</span><span class="sxs-lookup"><span data-stu-id="2bd25-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="2bd25-126">Anahtar yönetimi</span><span class="sxs-lookup"><span data-stu-id="2bd25-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="2bd25-127">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2bd25-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="2bd25-128">Bekleme durumunda anahtar şifreleme</span><span class="sxs-lookup"><span data-stu-id="2bd25-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="2bd25-129">Anahtar değiştirilemezliği ve ayarları değiştirme</span><span class="sxs-lookup"><span data-stu-id="2bd25-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="2bd25-130">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="2bd25-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="2bd25-131">Kısa ömürlü veri koruma sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2bd25-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="2bd25-132">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="2bd25-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="2bd25-133">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="2bd25-133">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="2bd25-134">ASP.NET’te <machineKey>‘i değiştirme</span><span class="sxs-lookup"><span data-stu-id="2bd25-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
