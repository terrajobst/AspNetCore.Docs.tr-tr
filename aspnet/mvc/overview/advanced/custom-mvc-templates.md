---
uid: mvc/overview/advanced/custom-mvc-templates
title: Özel MVC şablonu | Microsoft Docs
author: joeloff
description: Bir şablon bir VSIX uzantısı oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 715990e2d7151ad1ce8288169ef4bdd5c4243369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367250"
---
<a name="custom-mvc-template"></a><span data-ttu-id="7f3e6-103">Özel MVC şablonu</span><span class="sxs-lookup"><span data-stu-id="7f3e6-103">Custom MVC Template</span></span>
====================
<span data-ttu-id="7f3e6-104">tarafından [Jacques Eloff](https://github.com/joeloff)</span><span class="sxs-lookup"><span data-stu-id="7f3e6-104">by [Jacques Eloff](https://github.com/joeloff)</span></span>

<span data-ttu-id="7f3e6-105">MVC 3 araçları güncelleştirme sürümü Visual Studio 2010 için ayrı bir sihirbaz MVC projeleri için kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-105">The release of MVC 3 Tools Update for Visual Studio 2010 introduced a separate project wizard for MVC projects.</span></span> <span data-ttu-id="7f3e6-106">Değişiklik iki faktörlerden temelli.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-106">The change was driven by two factors.</span></span> <span data-ttu-id="7f3e6-107">İlk olarak, MVC 3 ve Razor gibi ek görünüm altyapıları için destek yeni şablonlar sunulmasıyla neden Visual Studio'da yeni proje iletişim kutusu overcrowding için.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-107">First, the introduction of new templates in MVC 3 and support for additional view engines such as Razor lead to overcrowding the New Project dialog in Visual Studio.</span></span> <span data-ttu-id="7f3e6-108">İkinci olarak, müşteriler için genişletilebilirlik noktaları isteyen ve yeni MVC projesi Sihirbazı bu isteklere yanıt fırsatı bize göze.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-108">Second, customers had been asking for extensibility points and the new MVC project wizard would afford us the opportunity to respond to these requests.</span></span>

<span data-ttu-id="7f3e6-109">Özel şablonlar ekleme yeni şablonlar MVC projesi Sihirbazı görünür yapmak için kayıt defterini kullanarak yararlandı işlemini merkezileştirir bir süreç oldu.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-109">Adding custom templates was an arduous process that relied on using the registry to make new templates visible to the MVC project wizard.</span></span> <span data-ttu-id="7f3e6-110">Yükleme sırasında gerekli kayıt defteri girdileri oluşturulacak emin olmak için bir MSI içinde sarmalamak yeni bir şablon yazarı vardı.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-110">The author of a new template had to wrap it inside an MSI to ensure that the necessary registry entries would be created at install time.</span></span> <span data-ttu-id="7f3e6-111">Kullanılabilir şablonu içeren bir ZIP dosyası yapmak ve gerekli kayıt defteri girdilerini el ile oluşturmak son olması için alternatif oluştu.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-111">The alternative was to make a ZIP file containing the template available and have the end-user create the required registry entries by hand.</span></span>

<span data-ttu-id="7f3e6-112">Tarafından sağlanan mevcut altyapınızı bazıları yararlanmak verdik için yukarıda sözü edilen yaklaşımlardan hiçbiri idealdir [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) Yazar kolaylaştırmak için uzantıları dağıtın ve MVC 4 ile başlayarak, özel MVC şablonları yükleyin Visual Studio 2012 için.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-112">Neither of the aforementioned approaches is ideal so we decided to leverage some of the existing infrastructure provided by [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensions to make it easier to author, distribute and install custom MVC templates starting with MVC 4 for Visual Studio 2012.</span></span> <span data-ttu-id="7f3e6-113">Bu yaklaşım tarafından sağlanan avantajlarından bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="7f3e6-113">Some of the benefits provided by this approach are:</span></span>

- <span data-ttu-id="7f3e6-114">Bir VSIX uzantısı, farklı diller (C# ve Visual Basic) destekleyen birden çok şablonları ve birden çok görünüm altyapısı (ASPX ve Razor) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-114">A VSIX extension can contain multiple templates that support different languages (C# and Visual Basic) and multiple view engines (ASPX and Razor).</span></span>
- <span data-ttu-id="7f3e6-115">Bir VSIX uzantısı, birden çok SKU'ları Visual Express SKU'ları da dahil olmak üzere Studio'nun hedefleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-115">A VSIX extension can target multiple SKUs of Visual Studio including Express SKUs.</span></span>
- <span data-ttu-id="7f3e6-116">[Visual Studio Galerisi](https://visualstudiogallery.msdn.microsoft.com/) uzantısı geniş bir kitleye dağıtmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-116">The [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilitates distributing the extension to a wide audience.</span></span>
- <span data-ttu-id="7f3e6-117">VSIX uzantılarını düzeltmeler ve güncelleştirmeler için Özel şablonlarınızı Yazar kolaylaştıran yükseltilebilir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-117">VSIX extensions can be upgraded making it easier to author corrections and updates to your custom templates.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f3e6-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7f3e6-118">Prerequisites</span></span>

- <span data-ttu-id="7f3e6-119">Kullanıcıların proje şablonları gerekli biçimlendirme vstemplate dosyaları, vb. dahil olmak üzere, yazma ile ilgili bilgi sahibi olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-119">Users need to be familiar with authoring project templates, including the required markup for vstemplate files, etc.</span></span>
- <span data-ttu-id="7f3e6-120">Kullanıcılar, Visual Studio Professional ayarlamış olmanız gerekir veya üzeri yüklü.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-120">Users will need to have Visual Studio Professional and higher installed.</span></span> <span data-ttu-id="7f3e6-121">VSIX proje oluşturma Express SKU'ları desteklemez.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-121">Express SKUs do not support creating VSIX projects.</span></span>
- <span data-ttu-id="7f3e6-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) yüklü.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installed.</span></span>

## <a name="example"></a><span data-ttu-id="7f3e6-123">Örnek</span><span class="sxs-lookup"><span data-stu-id="7f3e6-123">Example</span></span>

<span data-ttu-id="7f3e6-124">İlk adım, C# veya Visual Basic kullanarak yeni bir VSIX projesi oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-124">The first step is to create a new VSIX project using either C# or Visual Basic.</span></span> <span data-ttu-id="7f3e6-125">Seçin **Dosya > Yeni proje**, ardından **genişletilebilirlik** seçip sol bölmedeki **VSIX projesi**.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-125">Select **File > New Project**, then click **Extensibility** in the left pane and select the **VSIX Project**.</span></span>

![Yeni Proje](custom-mvc-templates/_static/image1.jpg)

<span data-ttu-id="7f3e6-127">Proje oluşturulduktan sonra VSIX Tasarımcısı açılır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-127">After the project is created, the VSIX designer will be opened.</span></span>

![Proje Tasarımcısı meta verileri](custom-mvc-templates/_static/image2.jpg)

<span data-ttu-id="7f3e6-129">Tasarımcı bazı uzantıyı yükleyin veya Visual Studio'da yüklü uzantılara Gözat yükleyen kullanıcılara gösterilen uzantısının genel özelliklerini düzenlemek için kullanılabilir (**Araçlar > Uzantılar ve güncelleştirmeler**).</span><span class="sxs-lookup"><span data-stu-id="7f3e6-129">The designer can be used to edit some of the general properties of the extension that will be shown to users when they install the extension or browse the installed extensions in Visual Studio (**Tools > Extensions and Updates**).</span></span> <span data-ttu-id="7f3e6-130">Genel bilgiler tamamladıktan sonra tıklayın **yükleme hedefler sekmesi**.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-130">Once you have completed the general information click on the **Install Targets tab**.</span></span>

![Proje Tasarımcısı yükleme hedefleri](custom-mvc-templates/_static/image3.jpg)

<span data-ttu-id="7f3e6-132">Bu sekme, SKU'ları ve uzantınız tarafından desteklenen Visual Studio sürümlerini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-132">This tab is used to specify the SKUs and versions of Visual Studio that are supported by your extension.</span></span> <span data-ttu-id="7f3e6-133">Onay kutusunu seçip **bu VSIX, tüm kullanıcılar için yüklenir** VSIX makine başına yüklemelerini etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-133">Select the checkbox for **This VSIX is installed for all users** to enable per-machine installs of the VSIX.</span></span> <span data-ttu-id="7f3e6-134">Tıklayarak **yeni** Web Developer Express (VWD) gibi ek SKU'ları eklemek için sağ taraftaki düğmeyi.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-134">Click on the **New** button on the right to add additional SKUs such as Web Developer Express (VWD).</span></span>

![Yeni yükleme hedefi ekleyin](custom-mvc-templates/_static/image4.jpg)

<span data-ttu-id="7f3e6-136">Tüm profesyonel ve daha yüksek SKU'ları (Professional, Premium ve Ultimate) desteklemek istiyorsanız, yalnızca en az bir SKU ailesi, seçmeniz gerekir **Microsoft.VisualStudio.Pro**.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-136">If you intend to support all the Professional and higher SKUs (Professional, Premium and Ultimate) you only need to select the minimum SKU in the family, **Microsoft.VisualStudio.Pro**.</span></span> <span data-ttu-id="7f3e6-137">Hedefleri yükle tamamladıktan sonra yaptığınız tüm değişiklikleri kaydetmeyi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-137">Remember to save all your changes once you have completed the Install Targets.</span></span>

![Proje Tasarımcısı yükleme hedefleri](custom-mvc-templates/_static/image5.jpg)

<span data-ttu-id="7f3e6-139">**Varlıklar** sekmesi VSIX'e tüm içerik dosyalarını eklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-139">The **Assets** tab is used to add all your content files to the VSIX.</span></span> <span data-ttu-id="7f3e6-140">MVC özel meta verileri gerektirdiğinden VSIX bildirim dosyası kullanmak yerine, ham XML düzenleyeceksiniz **varlıklar** içerik eklemek için sekmesinde.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-140">Since MVC requires custom metadata you will be editing the raw XML of the VSIX manifest file instead of using the **Assets** tab to add content.</span></span> <span data-ttu-id="7f3e6-141">VSIX projesi için şablon içeriği ekleyerek başlayın.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-141">Start by adding the template contents to the VSIX project.</span></span> <span data-ttu-id="7f3e6-142">Klasör ve içeriği yapısını proje düzenini yansıtma önemlidir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-142">It is important that the structure of the folder and the contents mirror the layout of the project.</span></span> <span data-ttu-id="7f3e6-143">Aşağıdaki örnek, temel MVC proje şablonundan türetilen dört proje şablonları içerir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-143">The example below contains four project templates that were derived from the Basic MVC project template.</span></span> <span data-ttu-id="7f3e6-144">İçin proje şablonu (her şeyi ProjectTemplates klasörü altında) oluşturan tüm dosyaların eklendiğinden emin olun **içerik** vsıx'te ItemGroup proje dosyası ve her öğenin içerdiğini  **CopyToOutputDirectory** ve **IncludeInVsix** meta veriler, aşağıdaki örnekte gösterildiği gibi ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-144">Make sure that all the files that comprise your project template (everything underneath the ProjectTemplates folder) are added to the **Content** itemgroup in the VSIX project file and that each item contains the **CopyToOutputDirectory** and **IncludeInVsix** metadata set as shown in the example below.</span></span>

<span data-ttu-id="7f3e6-145">&lt;İçerik =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span></span>

<span data-ttu-id="7f3e6-146">&lt;CopyToOutputDirectory&gt;her zaman&lt;/CopyToOutputDirectory&gt;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span></span>

<span data-ttu-id="7f3e6-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span></span>

<span data-ttu-id="7f3e6-148">&lt;/ İçerik&gt;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-148">&lt;/Content&gt;</span></span>

<span data-ttu-id="7f3e6-149">Aksi halde şablonunun içeriğini VSIX derleme ve büyük olasılıkla bir hata göreceğiniz derlemek IDE çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-149">If not, the IDE will try to compile the contents of the template when you build the VSIX and you will likely see an error.</span></span> <span data-ttu-id="7f3e6-150">Şablonları kod dosyaları genellikle içeren özel [şablon parametreleri](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) proje şablonu örneği ve IDE'de derlenemez Visual Studio tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-150">Code files in templates often contain special [template parameters](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) used by Visual Studio when the project template is instantiated and therefore cannot be compiled in the IDE.</span></span>

![Çözüm Gezgini](custom-mvc-templates/_static/image6.jpg)

<span data-ttu-id="7f3e6-152">VSIX Tasarımcısı'nı kapatın ve ardından sağ tıklayın **source.extension.manifest** dosyası **Çözüm Gezgini** seçip **birlikte Aç** ve **XML () Metin) Düzenleyicisi** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-152">Close the VSIX designer, then right click on the **source.extension.manifest** file in **Solution Explorer** and select **Open With** and choose the **XML (Text) Editor** option.</span></span>

![Aç iletişim kutusu](custom-mvc-templates/_static/image7.jpg)

<span data-ttu-id="7f3e6-154">Oluşturma bir **&lt;varlıklar&gt;** öğesi ve bir **&lt;varlık&gt;** öğesi içinde VSIX içine dahil her dosya için.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-154">Create an **&lt;Assets&gt;** element and add an **&lt;Asset&gt;** element for each file that must be included in the VSIX.</span></span> <span data-ttu-id="7f3e6-155">**Türü** her öznitelik **&lt;varlık&gt;** öğesi ayarlanmalıdır **Microsoft.VisualStudio.Mvc.Template**.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-155">The **Type** attribute of each **&lt;Asset&gt;** element must be set to **Microsoft.VisualStudio.Mvc.Template**.</span></span> <span data-ttu-id="7f3e6-156">Bu, yalnızca MVC projesi Sihirbazı anlayan bir özel ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-156">This is a custom namespace that only the MVC project wizard understands.</span></span> <span data-ttu-id="7f3e6-157">Bildirim dosyasının düzenini ve yapısı hakkında daha fazla bilgi için VSIX 2.0 şema belgelerine bakın.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-157">Refer to the VSIX 2.0 Schema documentation for additional information on the structure and layout of the manifest file.</span></span>

<span data-ttu-id="7f3e6-158">Yalnızca VSIX için dosyaları ekleme, şablonlar MVC Sihirbazı ile kaydetmek yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-158">Just adding the files to the VSIX is not sufficient to register the templates with the MVC wizard.</span></span> <span data-ttu-id="7f3e6-159">MVC sihirbazın şablon adı, açıklama, desteklenen bir görünüm altyapısı ve programlama dili gibi bilgiler sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-159">You need to provide information such as the template name, description, supported view engines and programming language to the MVC wizard.</span></span> <span data-ttu-id="7f3e6-160">Bu bilgiler ile ilişkili özel öznitelikler taşınan **&lt;varlık&gt;** her öğe **vstemplate** dosya.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-160">This information is carried in custom attributes associated with the **&lt;Asset&gt;** element for each **vstemplate** file.</span></span>

<span data-ttu-id="7f3e6-161">&lt;Varlık d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span></span>

<span data-ttu-id="7f3e6-162">Tür =&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span></span>

<span data-ttu-id="7f3e6-163">d:Source =&quot;dosyası&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-163">d:Source=&quot;File&quot;</span></span>

<span data-ttu-id="7f3e6-164">Yol =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span></span>

<span data-ttu-id="7f3e6-165">ProjectType=&quot;MVC&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-165">ProjectType=&quot;MVC&quot;</span></span>

<span data-ttu-id="7f3e6-166">Dil =&quot;C#&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-166">Language=&quot;C#&quot;</span></span>

<span data-ttu-id="7f3e6-167">ViewEngine =&quot;Aspx&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-167">ViewEngine=&quot;Aspx&quot;</span></span>

<span data-ttu-id="7f3e6-168">Templateıd =&quot;MyMvcApplication&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-168">TemplateId=&quot;MyMvcApplication&quot;</span></span>

<span data-ttu-id="7f3e6-169">Başlık =&quot;özel temel Web uygulaması&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-169">Title=&quot;Custom Basic Web Application&quot;</span></span>

<span data-ttu-id="7f3e6-170">Açıklama =&quot;özel bir şablon türetilmiş bir temel MVC web uygulaması (Razor)&quot;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-170">Description=&quot;A custom template derived from a Basic MVC web application (Razor)&quot;</span></span>

<span data-ttu-id="7f3e6-171">Sürüm =&quot;4.0&quot;/&gt;</span><span class="sxs-lookup"><span data-stu-id="7f3e6-171">Version=&quot;4.0&quot;/&gt;</span></span>

<span data-ttu-id="7f3e6-172">Aşağıda bulunması gereken özel özniteliklerin bir açıklaması verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7f3e6-172">Below is an explanation of the custom attributes that must be present:</span></span>

- <span data-ttu-id="7f3e6-173">**ProjectType** MVC için ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-173">**ProjectType** must be set to MVC.</span></span>
- <span data-ttu-id="7f3e6-174">**Dil** şablon tarafından desteklenen bir geliştirme dilini belirtir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-174">**Language** designates the development language supported by the template.</span></span> <span data-ttu-id="7f3e6-175">C# veya vb geçerli değerler:</span><span class="sxs-lookup"><span data-stu-id="7f3e6-175">Valid values are either C# or VB.</span></span>
- <span data-ttu-id="7f3e6-176">**ViewEngine** gibi Aspx veya Razor şablonu tarafından desteklenen görünüm altyapısını belirler.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-176">**ViewEngine** designates the view engine supported by the template such as Aspx or Razor.</span></span> <span data-ttu-id="7f3e6-177">Bu alan için özel bir değer belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-177">You can specify a custom value for this field.</span></span>
- <span data-ttu-id="7f3e6-178">**Templateıd** şablonları gruplandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-178">**TemplateId** is used for grouping the templates.</span></span> <span data-ttu-id="7f3e6-179">Bu, var olan bir şablon kimliği değerle eşleşiyorsa MVC Sihirbazı'nı kullanarak daha önce kaydetmiş şablonları geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-179">If the value matches an existing template ID it will be override templates previously registered with the MVC wizard.</span></span>
- <span data-ttu-id="7f3e6-180">**Başlık** MVC Sihirbazı her proje şablonunun altında görüntülenen kısa bir açıklama belirler.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-180">**Title** designates the short description displayed in the MVC wizard beneath each project template.</span></span>
- <span data-ttu-id="7f3e6-181">**Açıklama** şablon daha ayrıntılı bir açıklamasını belirtir.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-181">**Description** designates a more verbose description of the template.</span></span>

<span data-ttu-id="7f3e6-182">Bildirime tüm dosyaları ekledikten sonra kaydettiyseniz, göreceksiniz **varlıklar** tüm dosyaları tasarımcısında bir sekmeyi görüntüler, ancak eklediğiniz özel öznitelikleri **&lt;varlık&gt;** için öğeleri **vstemplate** dosyaları.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-182">After you have added all the files to the manifest and saved it, you will notice that the **Assets** tab in the designer will display all the files, but not the custom attributes you added to the **&lt;Asset&gt;** elements for the **vstemplate** files.</span></span>

![Proje Tasarımcısı varlıklar](custom-mvc-templates/_static/image8.jpg)

<span data-ttu-id="7f3e6-184">Kalan artık tek şey VSIX projesini derleme ve yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-184">All that remains now is to compile the VSIX project and install it.</span></span>

<span data-ttu-id="7f3e6-185">VSIX uzantıyı test etmek istediğiniz yere Visual Studio'nun tüm örneklerini makinede kapalı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-185">Make sure that all instances of Visual Studio are closed on the machine where you intend to test the VSIX extension.</span></span> <span data-ttu-id="7f3e6-186">Bir VSIX yüklenirken IDE açıksa Visual Studio'yu yeniden başlatmanız gerekecektir visual Studio için yeni uzantıları başlatma sırasında tarar.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-186">Visual Studio scans for new extensions during startup, so if the IDE is open while installing a VSIX you will need to restart Visual Studio.</span></span> <span data-ttu-id="7f3e6-187">Gezgini'nde başlatmak için VSIX dosyasını çift tıklatın **VSIX yükleyicisi**, tıklayın **yükleme** ve ardından Visual Studio'yu başlatın.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-187">In Explorer, double click on the VSIX file to launch the **VSIX Installer**, click **Install** and then launch Visual Studio.</span></span>

![VSIX yükleyicisi](custom-mvc-templates/_static/image9.jpg)

<span data-ttu-id="7f3e6-189">Menüden **Araçlar > Uzantılar ve güncelleştirmeler** uzantınızı yüklendiğini doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-189">From the menu, select **Tools > Extensions and Updates** to confirm that your extension was installed.</span></span> <span data-ttu-id="7f3e6-190">VSIX yükleyici, uzantının yüklenmesi sırasında hata bildirilirse, daha fazla bilgi için VSIX yükleyicisi bu günlüğü görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-190">If the VSIX Installer reported any errors during the installation of the extension you can view the VSIX Installer log for more information.</span></span> <span data-ttu-id="7f3e6-191">Günlük genellikle oluşturulan **% temp %** uzantısı, örneğin yüklü kullanıcı klasörüne **C:\Users\Bob\AppData\Local\Temp**.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-191">The log is usually created in the **%temp%** folder of the user that installed the extension, for example **C:\Users\Bob\AppData\Local\Temp**.</span></span>

![Uzantılar ve güncelleştirmeler](custom-mvc-templates/_static/image10.jpg)

<span data-ttu-id="7f3e6-193">Pencereyi kapattıktan sonra yeni şablonlarınızı MVC Sihirbazı'nda gösterilen olup olmadığını görmek için bir MVC 4 proje oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-193">After closing the window you can create an MVC 4 project to see whether your new templates are shown in the MVC wizard.</span></span>

![Yeni ASP.NET MVC 4 Proje](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a><span data-ttu-id="7f3e6-195">Sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="7f3e6-195">Limitations</span></span>

1. <span data-ttu-id="7f3e6-196">MVC Sihirbazı'nı yerelleştirilmiş özel şablonları desteklemez.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-196">The MVC wizard does not support localized custom templates.</span></span>
2. <span data-ttu-id="7f3e6-197">Özel şablonlar bulmak başarısız olursa, sihirbazın herhangi bir hata bildirmez.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-197">The wizard will not report any errors if it fails to locate custom templates.</span></span> <span data-ttu-id="7f3e6-198">Şablon, yalnızca gerekli özel öznitelikleri yoksa, Sihirbazı'ndan dışarıda.</span><span class="sxs-lookup"><span data-stu-id="7f3e6-198">If any of the required custom attributes are absent, the template would simply be excluded from the Wizard.</span></span>
