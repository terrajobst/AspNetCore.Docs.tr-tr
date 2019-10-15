---
title: ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı
author: guardrex
description: Çalışma zamanında yüklenen ad-değer çiftlerini kullanarak bir uygulamayı yapılandırmak için Azure Key Vault yapılandırma sağlayıcısını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: security/key-vault-configuration
ms.openlocfilehash: c8e76068dbcf2a59a15fa75a1fc5aa0032e6acc5
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334209"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="134d9-103">ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="134d9-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="134d9-104">[Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-nurte](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="134d9-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="134d9-105">Bu belgede, Azure Key Vault gizliliklerden uygulama yapılandırma değerlerini yüklemek için [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısının nasıl kullanılacağı açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="134d9-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="134d9-106">Azure Key Vault, uygulama ve hizmetler tarafından kullanılan şifreleme anahtarlarını ve gizli dizileri koruma konusunda yardımcı olan bulut tabanlı bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="134d9-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="134d9-107">ASP.NET Core uygulamalarla Azure Key Vault kullanmaya yönelik yaygın senaryolar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="134d9-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="134d9-108">Hassas yapılandırma verilerine erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="134d9-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="134d9-109">Yapılandırma verilerini depolarken FIPS 140-2 düzey 2 doğrulanan donanım güvenlik modülleri (HSM 'ler) gereksinimini karşılarsınız.</span><span class="sxs-lookup"><span data-stu-id="134d9-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="134d9-110">Bu senaryo, ASP.NET Core 2,1 veya sonraki bir sürümü hedefleyen uygulamalar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="134d9-111">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="134d9-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="134d9-112">Paketler</span><span class="sxs-lookup"><span data-stu-id="134d9-112">Packages</span></span>

<span data-ttu-id="134d9-113">Azure Key Vault yapılandırma sağlayıcısını kullanmak için [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="134d9-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="134d9-114">[Azure kaynakları Için Yönetilen kimlikler senaryosunu benimsemek](/azure/active-directory/managed-identities-azure-resources/overview) için [Microsoft. Azure. Services. appauthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="134d9-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="134d9-115">Yazma sırasında, `Microsoft.Azure.Services.AppAuthentication`, sürüm `1.0.3` ' in en son kararlı sürümü, [sistem tarafından atanan Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="134d9-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work).</span></span> <span data-ttu-id="134d9-116">@No__t-1 paketinde *Kullanıcı tarafından atanan Yönetilen kimlikler* için destek sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="134d9-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="134d9-117">Bu konu, sistem tarafından yönetilen kimliklerin kullanımını gösterir ve belirtilen örnek uygulama `Microsoft.Azure.Services.AppAuthentication` paketinin `1.0.3` sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="134d9-118">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="134d9-118">Sample app</span></span>

<span data-ttu-id="134d9-119">Örnek uygulama, *program.cs* dosyasının en üstünde `#define` ifadesiyle belirlenen iki moddan birinde çalışır:</span><span class="sxs-lookup"><span data-stu-id="134d9-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="134d9-120">`Certificate` &ndash; Azure Key Vault depolanan gizli dizileri erişmek için Azure Key Vault Istemci KIMLIĞI ve X. 509.952 sertifikası kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="134d9-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="134d9-121">Örneğin bu sürümü, Azure App Service dağıtılan herhangi bir konumdan veya ASP.NET Core uygulamasına hizmet veren herhangi bir konağa çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="134d9-122">`Managed` &ndash; Azure [kaynakları Için yönetilen kimliklerin](/azure/active-directory/managed-identities-azure-resources/overview) , uygulamanın kodunda veya yapılandırmasında depolanan kimlik bilgileri olmadan Azure AD kimlik doğrulamasıyla Azure Key Vault için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="134d9-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="134d9-123">Kimlik doğrulaması için Yönetilen kimlikler kullanıldığında, bir Azure AD uygulama KIMLIĞI ve parolası (gizli anahtar) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="134d9-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="134d9-124">Örneğin `Managed` sürümünün Azure 'a dağıtılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="134d9-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="134d9-125">[Azure kaynakları Için Yönetilen kimlikler bölümünü kullanma](#use-managed-identities-for-azure-resources) bölümündeki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="134d9-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="134d9-126">Önişlemci yönergeleri kullanarak örnek bir uygulamayı yapılandırma hakkında daha fazla bilgi için (`#define`), bkz. <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="134d9-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="134d9-127">Geliştirme ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="134d9-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="134d9-128">[Gizli dizi Yöneticisi aracını](xref:security/app-secrets)kullanarak gizli dizileri yerel olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="134d9-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="134d9-129">Örnek uygulama, geliştirme ortamındaki yerel makinede çalıştığında, gizli anahtar Yöneticisi deposundan gizli diziler yüklenir.</span><span class="sxs-lookup"><span data-stu-id="134d9-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="134d9-130">Gizli dizi Yöneticisi Aracı, uygulamanın proje dosyasında `<UserSecretsId>` özelliği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="134d9-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="134d9-131">Özellik değerini (`{GUID}`) herhangi bir benzersiz GUID 'ye ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="134d9-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="134d9-132">Gizlilikler ad-değer çiftleri olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="134d9-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="134d9-133">Hiyerarşik değerler (yapılandırma bölümleri) [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adlarında ayırıcı olarak `:` (iki nokta) kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="134d9-134">Gizli dizi, projenin [içerik köküne](xref:fundamentals/index#content-root)açılan bir komut kabuğundan kullanılır; burada `{SECRET NAME}` adı ve `{SECRET VALUE}` değeridir:</span><span class="sxs-lookup"><span data-stu-id="134d9-134">The Secret Manager is used from a command shell opened to the project's [content root](xref:fundamentals/index#content-root), where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="134d9-135">Örnek uygulamanın gizli dizilerini ayarlamak için, projenin [içerik kökünden](xref:fundamentals/index#content-root) bir komut kabuğu 'nda aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="134d9-135">Execute the following commands in a command shell from the project's [content root](xref:fundamentals/index#content-root) to set the secrets for the sample app:</span></span>

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="134d9-136">Bu gizlilikler, [Azure Key Vault bölümü Ile üretim ortamındaki gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) alanında Azure Key Vault depolandığında `_dev` son eki `_prod` olarak değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="134d9-137">Sonek, uygulamanın çıktısında yapılandırma değerlerinin kaynağını gösteren bir görsel ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="134d9-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="134d9-138">Azure Key Vault ile üretim ortamında gizli dizi</span><span class="sxs-lookup"><span data-stu-id="134d9-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="134d9-139">Hızlı başlangıç tarafından sunulan yönergeler [: Azure CLI kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](/azure/key-vault/quick-create-cli) , bir Azure Key Vault oluşturmak ve örnek uygulama tarafından kullanılan gizli dizileri depolamak için burada özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="134d9-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="134d9-140">Daha ayrıntılı bilgi için konusuna bakın.</span><span class="sxs-lookup"><span data-stu-id="134d9-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="134d9-141">[Azure Portal](https://portal.azure.com/)aşağıdaki yöntemlerden birini kullanarak Azure Cloud Shell 'i açın:</span><span class="sxs-lookup"><span data-stu-id="134d9-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="134d9-142">Bir kod bloğunun sağ üst köşesinde **deneyin** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="134d9-143">Metin kutusunda "Azure CLı" arama dizesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="134d9-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="134d9-144">**Cloud Shell Başlat** düğmesini kullanarak tarayıcınızda Cloud Shell açın.</span><span class="sxs-lookup"><span data-stu-id="134d9-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="134d9-145">Azure portal sağ üst köşesindeki menüdeki **Cloud Shell** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="134d9-146">Daha fazla bilgi için bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud Shell Genel Bakış](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="134d9-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="134d9-147">Henüz kimlik doğrulamasından geçirilmemişse `az login` komutuyla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="134d9-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="134d9-148">Aşağıdaki komutla bir kaynak grubu oluşturun; burada `{RESOURCE GROUP NAME}`, yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesidir (Datacenter):</span><span class="sxs-lookup"><span data-stu-id="134d9-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="134d9-149">Aşağıdaki komutla kaynak grubunda bir Anahtar Kasası oluşturun; burada `{KEY VAULT NAME}` Yeni Anahtar Kasası için addır ve `{LOCATION}` Azure bölgesidir (veri merkezi):</span><span class="sxs-lookup"><span data-stu-id="134d9-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="134d9-150">Anahtar kasasında ad-değer çiftleri olarak gizli diziler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="134d9-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="134d9-151">Azure Key Vault gizli dizi adları, alfasayısal karakterler ve tireler ile sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="134d9-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="134d9-152">Hiyerarşik değerler (yapılandırma bölümleri) ayırıcı olarak `--` (iki tire) kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="134d9-153">Genellikle [ASP.NET Core yapılandırmasındaki](xref:fundamentals/configuration/index)bir alt anahtardan bir bölümü sınırlandırmak için kullanılan iki nokta üst üste, Anahtar Kasası gizli adlarında izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="134d9-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="134d9-154">Bu nedenle, gizlilikler uygulamanın yapılandırmasına yüklendiğinde iki tire kullanılır ve iki nokta üst üste bulunur.</span><span class="sxs-lookup"><span data-stu-id="134d9-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="134d9-155">Aşağıdaki gizlilikler örnek uygulamayla kullanım içindir.</span><span class="sxs-lookup"><span data-stu-id="134d9-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="134d9-156">Değerler, Kullanıcı parolalarından geliştirme ortamında yüklenen `_dev` sonek değerlerinden ayırt etmek için bir `_prod` soneki içerir.</span><span class="sxs-lookup"><span data-stu-id="134d9-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="134d9-157">@No__t-0 ' yı önceki adımda oluşturduğunuz anahtar kasasının adıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="134d9-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="134d9-158">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanın</span><span class="sxs-lookup"><span data-stu-id="134d9-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="134d9-159">**Uygulama Azure dışında barındırıldığı zaman**bir anahtar kasasında kimlik doğrulaması yapmak IÇIN Azure AD, Azure Key Vault ve uygulamayı bir Azure ACTIVE DIRECTORY uygulama kimliği ve X. 509.440 sertifikası kullanacak şekilde yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="134d9-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="134d9-160">Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="134d9-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="134d9-161">Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanılması desteklenmekle birlikte, Azure 'da bir uygulama barındırılırken [Azure kaynakları Için Yönetilen kimlikler](#use-managed-identities-for-azure-resources) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="134d9-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="134d9-162">Yönetilen kimlikler, uygulamada veya geliştirme ortamında bir sertifikanın depolanmasını gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="134d9-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="134d9-163">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` açıklaması `Certificate` olarak ayarlandığında BIR uygulama kimliği ve X. 509.440 sertifikası kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="134d9-164">PKCS # 12 Arşivi ( *. pfx*) sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="134d9-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="134d9-165">Sertifika oluşturma seçenekleri Windows ve [OpenSSL](https://www.openssl.org/) [üzerinde MakeCert](/windows/desktop/seccrypto/makecert) içerir.</span><span class="sxs-lookup"><span data-stu-id="134d9-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="134d9-166">Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükler.</span><span class="sxs-lookup"><span data-stu-id="134d9-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="134d9-167">Anahtarı verilebilir olarak işaretlemek isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="134d9-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="134d9-168">Sertifikanın daha sonra bu işlemde kullanılan parmak izini aklınızda bulunur.</span><span class="sxs-lookup"><span data-stu-id="134d9-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="134d9-169">PKCS # 12 Arşivi ( *. pfx*) sertifikasını der kodlu bir sertifika ( *. cer*) olarak dışarı aktarın.</span><span class="sxs-lookup"><span data-stu-id="134d9-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="134d9-170">Uygulamayı Azure AD 'ye kaydedin (**uygulama kayıtları**).</span><span class="sxs-lookup"><span data-stu-id="134d9-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="134d9-171">DER kodlu sertifikayı ( *. cer*) Azure AD 'ye yükleyin:</span><span class="sxs-lookup"><span data-stu-id="134d9-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="134d9-172">Azure AD 'de uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="134d9-173">**Sertifikalar & gizli**dizi sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="134d9-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="134d9-174">Ortak anahtarı içeren sertifikayı karşıya yüklemek için **sertifikayı karşıya yükle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="134d9-175">*. Cer*, *. pek*veya *. CRT* sertifikası kabul edilebilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="134d9-176">Anahtar Kasası adı, uygulama KIMLIĞI ve sertifika parmak izini uygulamanın *appSettings. JSON* dosyasında depolayın.</span><span class="sxs-lookup"><span data-stu-id="134d9-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="134d9-177">Azure portal **ana** kasaları ' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="134d9-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="134d9-178">Azure Key Vault bölümünde, [üretim ortamındaki gizli dizi deposunda](#secret-storage-in-the-production-environment-with-azure-key-vault) oluşturduğunuz anahtar kasasını seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="134d9-179">**Erişim ilkeleri**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="134d9-180">**Yeni Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-180">Select **Add new**.</span></span>
1. <span data-ttu-id="134d9-181">**Sorumlu Seç** ' i seçin ve kayıtlı uygulamayı ada göre seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="134d9-182">**Seç** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="134d9-183">**Gizli izinleri** açın ve uygulamaya **Get** ve **list** izinleri sağlayın.</span><span class="sxs-lookup"><span data-stu-id="134d9-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="134d9-184">**Tamam ' ı**seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-184">Select **OK**.</span></span>
1. <span data-ttu-id="134d9-185">**Kaydet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="134d9-185">Select **Save**.</span></span>
1. <span data-ttu-id="134d9-186">Uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="134d9-186">Deploy the app.</span></span>

<span data-ttu-id="134d9-187">@No__t-0 örnek uygulaması, `IConfigurationRoot` ' den gelen yapılandırma değerlerini gizli adıyla aynı adla alır:</span><span class="sxs-lookup"><span data-stu-id="134d9-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="134d9-188">Hiyerarşik olmayan değerler: `SecretName` değeri `config["SecretName"]` ile elde edilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="134d9-189">Hiyerarşik değerler (bölümler): `:` (iki nokta üst üste) gösterimini veya `GetSection` uzantı yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="134d9-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="134d9-190">Yapılandırma değerini elde etmek için şu yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="134d9-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="134d9-191">X. 509.440 sertifikası işletim sistemi tarafından yönetiliyor.</span><span class="sxs-lookup"><span data-stu-id="134d9-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="134d9-192">Uygulama, *appSettings. JSON* dosyası tarafından sağlanan değerlerle `AddAzureKeyVault` ' yı çağırır:</span><span class="sxs-lookup"><span data-stu-id="134d9-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="134d9-193">Örnek değerler:</span><span class="sxs-lookup"><span data-stu-id="134d9-193">Example values:</span></span>

* <span data-ttu-id="134d9-194">Anahtar Kasası adı: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="134d9-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="134d9-195">Uygulama KIMLIĞI: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="134d9-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="134d9-196">Sertifika parmak izi: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="134d9-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="134d9-197">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="134d9-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="134d9-198">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="134d9-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="134d9-199">Geliştirme ortamında, gizli değerler `_dev` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="134d9-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="134d9-200">Üretim ortamında, değerler `_prod` soneki ile yüklenir.</span><span class="sxs-lookup"><span data-stu-id="134d9-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="134d9-201">Azure kaynakları için Yönetilen kimlikler kullanma</span><span class="sxs-lookup"><span data-stu-id="134d9-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="134d9-202">Azure **'a dağıtılan bir uygulama** , [Azure kaynakları için yönetilen kimliklerden](/azure/active-directory/managed-identities-azure-resources/overview)yararlanarak uygulamanın KIMLIK bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault kimlik doğrulaması yapmasına olanak sağlar (uygulama kimliği ve parola/istemci gizli anahtarı) uygulamada depolanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="134d9-203">Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` açıklaması `Managed` olarak ayarlandığında Azure kaynakları için Yönetilen kimlikler kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="134d9-204">Uygulamanın *appSettings. JSON* dosyasına kasa adını girin.</span><span class="sxs-lookup"><span data-stu-id="134d9-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="134d9-205">Örnek uygulama, `Managed` sürümüne ayarlandığında bir uygulama KIMLIĞI ve parolası (Istemci gizli dizisi) gerektirmez, bu nedenle bu yapılandırma girişlerini yoksayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="134d9-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="134d9-206">Uygulama Azure 'a dağıtılır ve Azure, yalnızca *appSettings. JSON* dosyasında depolanan kasa adını kullanarak Azure Key Vault erişmek için uygulamanın kimliğini doğrular.</span><span class="sxs-lookup"><span data-stu-id="134d9-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="134d9-207">Azure App Service için örnek uygulamayı dağıtın.</span><span class="sxs-lookup"><span data-stu-id="134d9-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="134d9-208">Azure App Service dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD 'ye otomatik olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="134d9-209">Aşağıdaki komutta kullanılmak üzere dağıtımdan nesne KIMLIĞINI edinin.</span><span class="sxs-lookup"><span data-stu-id="134d9-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="134d9-210">Nesne KIMLIĞI, App Service **kimlik** panelinde Azure Portal gösterilir.</span><span class="sxs-lookup"><span data-stu-id="134d9-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="134d9-211">Azure CLı ve uygulamanın nesne KIMLIĞINI kullanarak, anahtar kasasına erişmek için `list` ve `get` izinlerine sahip olan uygulamayı sağlayın:</span><span class="sxs-lookup"><span data-stu-id="134d9-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="134d9-212">Azure CLı, PowerShell veya Azure portal kullanarak **uygulamayı yeniden başlatın** .</span><span class="sxs-lookup"><span data-stu-id="134d9-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="134d9-213">Örnek uygulama:</span><span class="sxs-lookup"><span data-stu-id="134d9-213">The sample app:</span></span>

* <span data-ttu-id="134d9-214">Bağlantı dizesi olmadan `AzureServiceTokenProvider` sınıfının bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="134d9-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="134d9-215">Bir bağlantı dizesi sağlanmazsa, sağlayıcı Azure kaynakları için yönetilen kimliklerden bir erişim belirteci almaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="134d9-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="134d9-216">Yeni bir `KeyVaultClient` `AzureServiceTokenProvider` örnek belirteci geri çağırması ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="134d9-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="134d9-217">@No__t-0 örneği, tüm gizli değerleri yükleyen ve çift tireleri (`--`) anahtar adlarında iki nokta (`:`) ile değiştirir `IKeyVaultSecretManager` varsayılan uygulamasıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="134d9-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="134d9-218">Anahtar Kasası adı örnek değeri: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="134d9-218">Key vault name example value: `contosovault`</span></span>
    
<span data-ttu-id="134d9-219">*appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="134d9-219">*appsettings.json*:</span></span>

```json
{
  "KeyVaultName": "Key Vault Name"
}
```

<span data-ttu-id="134d9-220">Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="134d9-220">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="134d9-221">Geliştirme ortamında, gizli değerler `_dev` sonekine sahiptir çünkü Kullanıcı gizli dizileri tarafından sağlanırlar.</span><span class="sxs-lookup"><span data-stu-id="134d9-221">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="134d9-222">Üretim ortamında, Azure Key Vault tarafından sağlandıklarından, değerler `_prod` sonekiyle yüklenir.</span><span class="sxs-lookup"><span data-stu-id="134d9-222">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="134d9-223">@No__t-0 hatası alırsanız, uygulamanın Azure AD 'ye kayıtlı olduğunu ve anahtar kasasına erişim sağladıklarını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="134d9-223">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="134d9-224">Azure 'da hizmeti yeniden başlattığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="134d9-224">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="134d9-225">Anahtar adı öneki kullanın</span><span class="sxs-lookup"><span data-stu-id="134d9-225">Use a key name prefix</span></span>

<span data-ttu-id="134d9-226">`AddAzureKeyVault`, Anahtar Kasası gizli dizilerini yapılandırma anahtarlarına nasıl dönüştürdüğünü denetlemenize olanak sağlayan `IKeyVaultSecretManager` uygulamasını kabul eden bir aşırı yükleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="134d9-226">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="134d9-227">Örneğin, uygulama başlangıcında sağladığınız önek değerine göre gizli değerleri yüklemek için arabirimini uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="134d9-227">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="134d9-228">Bu, örneğin, uygulama sürümüne göre gizli dizileri yüklemeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="134d9-228">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="134d9-229">Birden çok uygulama için gizli dizileri aynı kasaya yerleştirmek veya çevresel gizli dizileri (örneğin, *geliştirme* ve *Üretim* gizlilikleri) aynı kasaya yerleştirmek için Anahtar Kasası gizli dizileri üzerinde ön ekleri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="134d9-229">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="134d9-230">Farklı uygulama ve geliştirme/üretim ortamlarının, uygulama ortamlarını en yüksek düzeyde güvenlik için yalıtmak üzere ayrı anahtar kasaları kullanmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="134d9-230">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="134d9-231">Aşağıdaki örnekte, anahtar kasasında (ve geliştirme ortamı için gizli Yönetici Aracı kullanılarak) `5000-AppSecret` (Anahtar Kasası gizli adlarında döneme izin verilmez) için bir gizli dizi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="134d9-231">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="134d9-232">Bu gizli anahtar, uygulamanın 5.0.0.0 sürümü için bir uygulama gizli anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="134d9-232">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="134d9-233">Uygulamanın başka bir sürümü olan 5.1.0.0, anahtar kasasına (ve gizli Yönetici Aracı kullanılarak) `5100-AppSecret` için bir gizli dizi eklenir.</span><span class="sxs-lookup"><span data-stu-id="134d9-233">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="134d9-234">Her bir uygulama sürümü sürümlenmiş gizli değerini `AppSecret` olarak yükler ve gizli anahtarı yüklerken sürümü kapatır.</span><span class="sxs-lookup"><span data-stu-id="134d9-234">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="134d9-235">`AddAzureKeyVault`, özel bir `IKeyVaultSecretManager` ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="134d9-235">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="134d9-236">@No__t 0 uygulama, doğru gizli anahtarı yapılandırmaya yüklemek için gizli dizi sürüm öneklerine yeniden davranır:</span><span class="sxs-lookup"><span data-stu-id="134d9-236">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="134d9-237">@No__t-0 yöntemi, sürüm ön ekine sahip olanları bulmak için kasa gizli dizileri aracılığıyla yinelenen bir sağlayıcı algoritması tarafından çağırılır.</span><span class="sxs-lookup"><span data-stu-id="134d9-237">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="134d9-238">@No__t-0 ile bir sürüm ön eki bulunduğunda, algoritma, parola adının yapılandırma adını döndürmek için `GetKey` yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-238">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="134d9-239">Gizlilik adından sürüm önekini kaldırır ve uygulamanın yapılandırma adı-değer çiftlerine yüklemek için gizli dizi adının geri kalanını döndürür.</span><span class="sxs-lookup"><span data-stu-id="134d9-239">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="134d9-240">Bu yaklaşım uygulandığında:</span><span class="sxs-lookup"><span data-stu-id="134d9-240">When this approach is implemented:</span></span>

1. <span data-ttu-id="134d9-241">Uygulamanın proje dosyasında belirtilen sürümü.</span><span class="sxs-lookup"><span data-stu-id="134d9-241">The app's version specified in the app's project file.</span></span> <span data-ttu-id="134d9-242">Aşağıdaki örnekte, uygulamanın sürümü `5.0.0.0` olarak ayarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="134d9-242">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="134d9-243">Uygulamanın proje dosyasında `<UserSecretsId>` özelliğinin bulunduğunu doğrulayın; burada `{GUID}` Kullanıcı tarafından sağlanan bir GUID 'dir:</span><span class="sxs-lookup"><span data-stu-id="134d9-243">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="134d9-244">[Gizli dizi yöneticisi aracıyla](xref:security/app-secrets)aşağıdaki gizli dizileri yerel olarak kaydedin:</span><span class="sxs-lookup"><span data-stu-id="134d9-244">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="134d9-245">Gizlilikler, aşağıdaki Azure CLı komutları kullanılarak Azure Key Vault kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="134d9-245">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="134d9-246">Uygulama çalıştırıldığında, Anahtar Kasası gizli dizileri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="134d9-246">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="134d9-247">@No__t-0 için dize gizli dizisi, uygulamanın proje dosyasında belirtilen sürümle (`5.0.0.0`) eşleşir.</span><span class="sxs-lookup"><span data-stu-id="134d9-247">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="134d9-248">@No__t-0 (Dash ile) sürümü, anahtar adından çıkarılır.</span><span class="sxs-lookup"><span data-stu-id="134d9-248">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="134d9-249">Uygulamanın tamamında, `AppSecret` anahtarıyla yapılandırmayı okumak gizli değeri yükler.</span><span class="sxs-lookup"><span data-stu-id="134d9-249">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="134d9-250">Uygulamanın sürümü proje dosyasında `5.1.0.0` olarak değiştirilirse ve uygulama yeniden çalıştırıldığında, döndürülen gizli değer geliştirme ortamında ve üretimde `5.1.0.0_secret_value_prod` ' dir `5.1.0.0_secret_value_dev` ' dir.</span><span class="sxs-lookup"><span data-stu-id="134d9-250">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="134d9-251">Ayrıca, `AddAzureKeyVault` için kendi `KeyVaultClient` uygulamanızı sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="134d9-251">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="134d9-252">Özel istemci, uygulama genelinde istemcinin tek bir örneğini paylaşıma izin verir.</span><span class="sxs-lookup"><span data-stu-id="134d9-252">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="134d9-253">Bir diziyi sınıfa bağlama</span><span class="sxs-lookup"><span data-stu-id="134d9-253">Bind an array to a class</span></span>

<span data-ttu-id="134d9-254">Sağlayıcı, bir POCO dizisine bağlamak için yapılandırma değerlerini bir diziye okuyabilme özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="134d9-254">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="134d9-255">Anahtarların iki nokta (`:`) ayırıcılar içermesine izin veren bir yapılandırma kaynağından okurken, bir diziyi oluşturan anahtarları ayırt etmek için bir sayısal anahtar kesimi kullanılır (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="134d9-255">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="134d9-256">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="134d9-256">`:{n}:`).</span></span> <span data-ttu-id="134d9-257">Daha fazla bilgi için bkz. [yapılandırma: diziyi bir sınıfa bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="134d9-257">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="134d9-258">Azure Key Vault anahtarlar ayırıcı olarak iki nokta üst üste kullanamaz.</span><span class="sxs-lookup"><span data-stu-id="134d9-258">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="134d9-259">Bu konuda açıklanan yaklaşım, hiyerarşik değerler (bölümler) için bir ayırıcı olarak çift tire (`--`) kullanır.</span><span class="sxs-lookup"><span data-stu-id="134d9-259">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="134d9-260">Dizi anahtarları Çift tire ve sayısal anahtar kesimlerle Azure Key Vault depolanır (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="134d9-260">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="134d9-261">Bir JSON dosyası tarafından sunulan aşağıdaki [Serilog](https://serilog.net/) günlük sağlayıcısı yapılandırmasını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="134d9-261">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="134d9-262">@No__t-0 dizisinde, günlük çıkışı için hedefleri açıklayan *Iki Serilog girişi*yansıtan iki nesne sabit değeri mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="134d9-262">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="134d9-263">Önceki JSON dosyasında gösterilen yapılandırma Çift tire (`--`) gösterimi ve sayısal kesimleri kullanılarak Azure Key Vault depolanır:</span><span class="sxs-lookup"><span data-stu-id="134d9-263">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="134d9-264">Anahtar</span><span class="sxs-lookup"><span data-stu-id="134d9-264">Key</span></span> | <span data-ttu-id="134d9-265">Değer</span><span class="sxs-lookup"><span data-stu-id="134d9-265">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="134d9-266">Gizli dizileri yeniden yükleme</span><span class="sxs-lookup"><span data-stu-id="134d9-266">Reload secrets</span></span>

<span data-ttu-id="134d9-267">Gizlilikler `IConfigurationRoot.Reload()` çağrılana kadar önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="134d9-267">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="134d9-268">Anahtar kasasındaki süre sonu, devre dışı ve güncelleştirilmiş gizli dizileri `Reload` yürütülene kadar uygulama tarafından dikkate alınır.</span><span class="sxs-lookup"><span data-stu-id="134d9-268">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="134d9-269">Devre dışı ve süre dolma parolaları</span><span class="sxs-lookup"><span data-stu-id="134d9-269">Disabled and expired secrets</span></span>

<span data-ttu-id="134d9-270">Devre dışı ve süresi biten gizlilikler, çalışma zamanında bir `KeyVaultClientException` oluşturur.</span><span class="sxs-lookup"><span data-stu-id="134d9-270">Disabled and expired secrets throw a `KeyVaultClientException` at runtime.</span></span> <span data-ttu-id="134d9-271">Uygulamanın üretilmesini engellemek için, farklı bir yapılandırma sağlayıcısı kullanarak yapılandırmayı sağlayın veya devre dışı ya da süre dolma parolasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="134d9-271">To prevent the app from throwing, provide the configuration using a different configuration provider or update the disabled or expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="134d9-272">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="134d9-272">Troubleshoot</span></span>

<span data-ttu-id="134d9-273">Uygulama, sağlayıcıyı kullanarak yapılandırmayı yükleyemediğinde, [ASP.NET Core günlük altyapısına](xref:fundamentals/logging/index)bir hata iletisi yazılır.</span><span class="sxs-lookup"><span data-stu-id="134d9-273">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="134d9-274">Aşağıdaki koşullar yapılandırmanın yüklenmesine engel olur:</span><span class="sxs-lookup"><span data-stu-id="134d9-274">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="134d9-275">Uygulama veya sertifika Azure Active Directory içinde doğru yapılandırılmamış.</span><span class="sxs-lookup"><span data-stu-id="134d9-275">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="134d9-276">Anahtar Kasası Azure Key Vault içinde yok.</span><span class="sxs-lookup"><span data-stu-id="134d9-276">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="134d9-277">Uygulamanın anahtar kasasına erişme yetkisi yok.</span><span class="sxs-lookup"><span data-stu-id="134d9-277">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="134d9-278">Erişim ilkesi `Get` ve `List` izinleri içermez.</span><span class="sxs-lookup"><span data-stu-id="134d9-278">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="134d9-279">Anahtar kasasında yapılandırma verileri (ad-değer çifti) yanlış olarak adlandırılmış, eksik, devre dışı veya zaman aşımına uğradı.</span><span class="sxs-lookup"><span data-stu-id="134d9-279">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="134d9-280">Uygulamanın Anahtar Kasası adı (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`) veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`) vardır.</span><span class="sxs-lookup"><span data-stu-id="134d9-280">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="134d9-281">Yüklemeye çalıştığınız değer için uygulamada yapılandırma anahtarı (ad) yanlış.</span><span class="sxs-lookup"><span data-stu-id="134d9-281">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="134d9-282">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="134d9-282">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="134d9-283">Microsoft Azure: Key Vault</span><span class="sxs-lookup"><span data-stu-id="134d9-283">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="134d9-284">Microsoft Azure: Key Vault belgeler</span><span class="sxs-lookup"><span data-stu-id="134d9-284">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="134d9-285">Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma</span><span class="sxs-lookup"><span data-stu-id="134d9-285">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="134d9-286">KeyVaultClient sınıfı</span><span class="sxs-lookup"><span data-stu-id="134d9-286">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="134d9-287">Hızlı başlangıç: .NET Web uygulaması kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma</span><span class="sxs-lookup"><span data-stu-id="134d9-287">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="134d9-288">Öğretici: .NET 'te Azure Windows sanal makinesi ile Azure Key Vault kullanma</span><span class="sxs-lookup"><span data-stu-id="134d9-288">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
