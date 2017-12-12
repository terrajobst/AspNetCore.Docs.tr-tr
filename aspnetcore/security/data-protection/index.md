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
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="d955e-104">ASP.NET Çekirdeği'nde veri koruma: tüketici API'leri, yapılandırma, genişletilebilirlik API'leri ve uygulama</span><span class="sxs-lookup"><span data-stu-id="d955e-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="d955e-105">Veri koruma giriş</span><span class="sxs-lookup"><span data-stu-id="d955e-105">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="d955e-106">Veri Koruma API'ları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d955e-106">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="d955e-107">Tüketici API'leri</span><span class="sxs-lookup"><span data-stu-id="d955e-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="d955e-108">Tüketici API'leri genel bakış</span><span class="sxs-lookup"><span data-stu-id="d955e-108">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="d955e-109">Amaç dizeleri</span><span class="sxs-lookup"><span data-stu-id="d955e-109">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="d955e-110">Amaç hiyerarşi ve çoklu kiracı</span><span class="sxs-lookup"><span data-stu-id="d955e-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="d955e-111">Parola karma</span><span class="sxs-lookup"><span data-stu-id="d955e-111">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="d955e-112">Korumalı yüklerini ömrü sınırlama</span><span class="sxs-lookup"><span data-stu-id="d955e-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="d955e-113">Kaldırmayı yükü, anahtarları iptal edildi</span><span class="sxs-lookup"><span data-stu-id="d955e-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="d955e-114">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d955e-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="d955e-115">Veri korumasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d955e-115">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="d955e-116">Varsayılan ayarları</span><span class="sxs-lookup"><span data-stu-id="d955e-116">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="d955e-117">Makine geniş İlkesi</span><span class="sxs-lookup"><span data-stu-id="d955e-117">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="d955e-118">Olmayan dı kullanan senaryolar</span><span class="sxs-lookup"><span data-stu-id="d955e-118">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="d955e-119">Genişletilebilirlik API</span><span class="sxs-lookup"><span data-stu-id="d955e-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="d955e-120">Çekirdek şifreleme genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="d955e-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="d955e-121">Anahtar Yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="d955e-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="d955e-122">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="d955e-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="d955e-123">Uygulama</span><span class="sxs-lookup"><span data-stu-id="d955e-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="d955e-124">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="d955e-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="d955e-125">Alt anahtar türetme ve kimliği doğrulanmış şifreleme</span><span class="sxs-lookup"><span data-stu-id="d955e-125">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="d955e-126">İçerik üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="d955e-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="d955e-127">Anahtar Yönetimi</span><span class="sxs-lookup"><span data-stu-id="d955e-127">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="d955e-128">Anahtar depolama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="d955e-128">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="d955e-129">REST anahtar şifrelemesi</span><span class="sxs-lookup"><span data-stu-id="d955e-129">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="d955e-130">Anahtar girişi ve ayarlarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="d955e-130">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="d955e-131">Anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="d955e-131">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="d955e-132">Kısa ömürlü veri koruma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="d955e-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="d955e-133">Uyumluluk</span><span class="sxs-lookup"><span data-stu-id="d955e-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="d955e-134">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="d955e-134">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="d955e-135">Değiştirme <machineKey> ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d955e-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
