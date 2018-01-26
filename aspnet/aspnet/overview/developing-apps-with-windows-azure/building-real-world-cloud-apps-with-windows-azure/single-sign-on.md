---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: "Çoklu oturum açma (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: b3640c94a8ae9ede330c0fe6a392acb5843cb65c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="e687c-104">Çoklu oturum açma (Azure ile gerçek bulut uygulamaları derleme)</span><span class="sxs-lookup"><span data-stu-id="e687c-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="e687c-105">tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e687c-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e687c-106">[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="e687c-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="e687c-107">**Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır.</span><span class="sxs-lookup"><span data-stu-id="e687c-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="e687c-108">13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı.</span><span class="sxs-lookup"><span data-stu-id="e687c-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="e687c-109">E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e687c-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="e687c-110">Bir bulut uygulaması zaman geliştirirken hakkında düşünmek için çok sayıda güvenlik sorunları vardır, ancak bu seri için biz yalnızca birinde odaklanmak: çoklu oturum açma.</span><span class="sxs-lookup"><span data-stu-id="e687c-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="e687c-111">Bir soru kişiler genellikle sorun şudur: "ı öncelikle uygulamaları Şirketim; çalışanlar için derleme nasıl bulutta bu uygulamaları barındırmak ve bunları my çalışanlar bilmeniz ve şirket içi ortamda bunlar uygulamalar, çalıştırırken kullanan aynı güvenlik modelini kullanmak hala etkinleştirmek barındırıldığı güvenlik duvarı içindeki?"</span><span class="sxs-lookup"><span data-stu-id="e687c-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="e687c-112">Bu senaryo etkinleştirme yollarından biri, Azure Active Directory (Azure AD) adı verilir.</span><span class="sxs-lookup"><span data-stu-id="e687c-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="e687c-113">Azure AD kuruluş satır iş kolu (LOB) uygulamaları Internet üzerinden kullanılabilmesini sağlar ve bu uygulamaları da iş ortakları için kullanılabilir olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="e687c-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="e687c-114">Azure AD giriş</span><span class="sxs-lookup"><span data-stu-id="e687c-114">Introduction to Azure AD</span></span>

<span data-ttu-id="e687c-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) sağlar [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) bulutta.</span><span class="sxs-lookup"><span data-stu-id="e687c-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="e687c-116">Temel özellikleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="e687c-116">Key features include the following:</span></span>

- <span data-ttu-id="e687c-117">Şirket içi Active Directory ile tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="e687c-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="e687c-118">Çoklu oturum açma uygulamalarınızda sağlar.</span><span class="sxs-lookup"><span data-stu-id="e687c-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="e687c-119">Gibi açık standartlar destekleyen [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), ve [OAuth 2.0](http://oauth.net/2/).</span><span class="sxs-lookup"><span data-stu-id="e687c-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="e687c-120">Kurumsal destekleyen [grafik REST API'si](https://msdn.microsoft.com/library/hh974476.aspx).</span><span class="sxs-lookup"><span data-stu-id="e687c-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="e687c-121">Intranet uygulamalarını imzalamak çalışanlar için kullandığınız bir şirket içi Windows Server Active Directory ortamına olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="e687c-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="e687c-122">Hangi Azure AD yapmanıza olanak sağlar, bulutta bir dizin oluşturun olur.</span><span class="sxs-lookup"><span data-stu-id="e687c-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="e687c-123">Boş bir özelliktir ve ayarlamak kolay.</span><span class="sxs-lookup"><span data-stu-id="e687c-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="e687c-124">Şirket içi Active Directory'nizden tamamen bağımsız olabilir; Herkes içinde istediğiniz ve bunları Internet uygulamalarında kimlik doğrulaması koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="e687c-126">Veya şirket içi ile tümleştirmek AD.</span><span class="sxs-lookup"><span data-stu-id="e687c-126">Or you can integrate it with your on-premises AD.</span></span>

![AD ve WAAD tümleştirme](single-sign-on/_static/image3.png)

<span data-ttu-id="e687c-128">Şimdi şirket içi kimlik doğrulaması tüm çalışanlar aynı zamanda Internet üzerinden – bir Güvenlik Duvarı'nı açın veya veri merkezinizdeki herhangi bir yeni sunucu dağıtmak zorunda kalmadan kimlik doğrulaması yapabilir.</span><span class="sxs-lookup"><span data-stu-id="e687c-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="e687c-129">Bilmeniz ve dahili uygulamalara çoklu oturum yeteneğini vermek için bugün kullanan tüm mevcut Active Directory ortamında yüklemenizden yararlanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="e687c-130">AD ve Azure AD arasındaki bu bağlantı yapmış olduğunuz sonra web uygulamaları ve mobil cihazlarınızı çalışanlarınızın bulut kimlik doğrulaması için de etkinleştirebilirsiniz ve kabul etmek için Office 365, SalesForce.com veya Google apps gibi üçüncü taraf uygulamaları etkinleştirebilirsiniz, çalışanların kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="e687c-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="e687c-131">Office 365 kullanıyorsanız, Office 365 Azure AD kimlik doğrulama ve yetkilendirme için kullandığı için Azure AD ile ayarlamış.</span><span class="sxs-lookup"><span data-stu-id="e687c-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![3 taraf uygulamalar](single-sign-on/_static/image4.png)

<span data-ttu-id="e687c-133">Güzelliği bu yaklaşım, kuruluşunuzun ekler veya bir kullanıcıyı siler dilediğiniz zaman olan veya bir kullanıcı bir parola değişikliklerini, şirket içi ortamınızda günümüzde kullandığınız aynı işlemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="e687c-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="e687c-134">Tüm şirket içi AD değişiklikleri otomatik olarak bulut ortamı yayılır.</span><span class="sxs-lookup"><span data-stu-id="e687c-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="e687c-135">Office 365 Azure AD kimlik doğrulaması için kullandığı için otomatik olarak ayarlanan Azure AD, şirketinizin kullanarak veya iyi haber olan Office 365'e taşıma gerekir.</span><span class="sxs-lookup"><span data-stu-id="e687c-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="e687c-136">Bu nedenle, kendi uygulamalarında kolayca Office 365 kullanan kimlik doğrulaması kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="e687c-137">Azure AD kiracısı ayarlayın</span><span class="sxs-lookup"><span data-stu-id="e687c-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="e687c-138">Azure AD dizini Azure AD adlı [Kiracı](https://technet.microsoft.com/library/jj573650.aspx), ve bir kiracı ayardır oldukça kolaydır.</span><span class="sxs-lookup"><span data-stu-id="e687c-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="e687c-139">Kavramları göstermek için Azure Yönetim Portalı'nda nasıl yapıldığını göstereceğiz, ancak Elbette gibi diğer portal işlevler ayrıca bir komut dosyası veya yönetim API'si kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="e687c-140">Yönetim Portalı'nda Active Directory sekmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="e687c-140">In the management portal click the Active Directory tab.</span></span>

![Portalı'nda WAAD](single-sign-on/_static/image5.png)

<span data-ttu-id="e687c-142">Azure hesabınız için otomatik olarak bir Azure AD kiracısı varsa ve tıklayabilirsiniz **Ekle** ek dizinler oluşturmak için sayfanın altındaki düğmesini.</span><span class="sxs-lookup"><span data-stu-id="e687c-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="e687c-143">Sınama ortamı için diğeri için üretim, örneğin isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="e687c-144">Yeni bir dizin adı hakkında dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="e687c-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="e687c-145">Dizin için adınızı kullanın ve ardından adınızı yeniden kullanıcıların biri için kullanıyorsanız, kafa karıştırıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="e687c-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![Dizin Ekle](single-sign-on/_static/image6.png)

<span data-ttu-id="e687c-147">Portal oluşturma, silme ve bu ortam içinde kullanıcıları yönetmek için tam destek sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e687c-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="e687c-148">Örneğin, eklemek için bir kullanıcı Git **kullanıcılar** sekmesinde **Kullanıcı Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e687c-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![Kullanıcı Ekle düğmesi](single-sign-on/_static/image7.png)

![Kullanıcı iletişim ekleyin](single-sign-on/_static/image8.png)

<span data-ttu-id="e687c-151">Yalnızca bu dizinde var. yeni bir kullanıcı oluşturabilir veya, bir Microsoft Account bu dizini ya da kaydı bir kullanıcı veya başka bir Azure AD dizininden bir kullanıcı olarak bu dizindeki bir kullanıcı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="e687c-152">(Gerçek bir dizinde ContosoTest.onmicrosoft.com varsayılan etki alanı olacaktır. Ayrıca kendi, contoso.com gibi seçme bir etki alanı kullanabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="e687c-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com. You can also use a domain of your own choosing, like contoso.com.)</span></span>

![Kullanıcı türleri](single-sign-on/_static/image9.png)

![Kullanıcı iletişim ekleyin](single-sign-on/_static/image10.png)

<span data-ttu-id="e687c-155">Kullanıcı rol atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-155">You can assign the user to a role.</span></span>

![Kullanıcı profili](single-sign-on/_static/image11.png)

<span data-ttu-id="e687c-157">Ve hesabın bir geçici parola oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e687c-157">And the account is created with a temporary password.</span></span>

![Geçici parola](single-sign-on/_static/image12.png)

<span data-ttu-id="e687c-159">Bu şekilde oluşturmalarını hemen bu bulut dizini kullanarak, web uygulamalarınızı oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="e687c-159">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="e687c-160">Ancak, Kurumsal çoklu oturum açma için harika nedir olan **dizin tümleştirme** sekmesi:</span><span class="sxs-lookup"><span data-stu-id="e687c-160">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![Dizin tümleştirme sekmesi](single-sign-on/_static/image13.png)

<span data-ttu-id="e687c-162">Dizin tümleştirmesi etkinleştirirseniz ve [bir aracı yükleyin](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), bu bulut dizini mevcut şirket içi Active directory'nizle kuruluşunuz içinde zaten kullanmakta olduğunuz eşitleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-162">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="e687c-163">Ardından, dizinde saklanan tüm kullanıcıları bu bulut dizinde görünecektir.</span><span class="sxs-lookup"><span data-stu-id="e687c-163">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="e687c-164">Bulut uygulamalarınızı şimdi tüm çalışanlarınız, mevcut Active Directory kimlik bilgilerini kullanarak kimlik doğrulaması yapabilir.</span><span class="sxs-lookup"><span data-stu-id="e687c-164">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="e687c-165">Ve tüm bu boş – eşitleme aracı ve Azure AD kendisi.</span><span class="sxs-lookup"><span data-stu-id="e687c-165">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="e687c-166">Bu ekran görüntüleri görebileceğiniz gibi kullanımı kolay, bir sihirbaz aracıdır.</span><span class="sxs-lookup"><span data-stu-id="e687c-166">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="e687c-167">Bu yönergelerin, yalnızca temel işlem gösteren bir örnek değildir.</span><span class="sxs-lookup"><span data-stu-id="e687c-167">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="e687c-168">Bağlantıları ayrıntılı how-to--BT bilgi için bkz: [kaynakları](#resources) bölümün sonunda bölüm.</span><span class="sxs-lookup"><span data-stu-id="e687c-168">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image14.png)

<span data-ttu-id="e687c-170">Tıklatın **sonraki**ve ardından Azure Active Directory kimlik bilgilerinizi girin.</span><span class="sxs-lookup"><span data-stu-id="e687c-170">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image15.png)

<span data-ttu-id="e687c-172">Tıklatın **sonraki**ve şirket içi enter AD kimlik.</span><span class="sxs-lookup"><span data-stu-id="e687c-172">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image16.png)

<span data-ttu-id="e687c-174">Tıklatın **sonraki**ve ardından AD parolalarınızı karmasını bulutta depolamak istiyorsanız, belirtin.</span><span class="sxs-lookup"><span data-stu-id="e687c-174">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image17.png)

<span data-ttu-id="e687c-176">Bulutta depolayabilir parola karması tek yönlü karma olur; Gerçek parola hiçbir zaman Azure AD içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="e687c-176">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="e687c-177">Karma buluta depolamaya karşı karar verirseniz, kullanmanız gerekir [Active Directory Federasyon Hizmetleri](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span><span class="sxs-lookup"><span data-stu-id="e687c-177">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="e687c-178">Ayrıca [dikkate almanız gereken diğer faktörleri ADFS kullanılıp kullanılmayacağını seçme](https://technet.microsoft.com/library/jj573653.aspx).</span><span class="sxs-lookup"><span data-stu-id="e687c-178">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="e687c-179">ADFS seçeneği birkaç ek yapılandırma adımları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e687c-179">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="e687c-180">Bulutta karma değerlerini depolamak tercih ederseniz, işiniz bittiğinde ve tıkladığınızda dizin eşitleme Aracı'nı başlatır **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="e687c-180">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image18.png)

<span data-ttu-id="e687c-182">Ve birkaç dakika içinde bitirdiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-182">And in a few minutes you're done.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image19.png)

<span data-ttu-id="e687c-184">Yalnızca bu kuruluşunuzdaki Windows 2003 veya daha yüksek bir etki alanı denetleyicisinde çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e687c-184">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="e687c-185">Ve yeniden başlatma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e687c-185">And no need to reboot.</span></span> <span data-ttu-id="e687c-186">İşiniz bittiğinde, kullanıcılarınızın bulutta tümü ve yapabileceğiniz herhangi bir web veya mobil uygulama, SAML, OAuth veya WS-Fed kullanarak çoklu oturum açma.</span><span class="sxs-lookup"><span data-stu-id="e687c-186">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="e687c-187">Bazen biz budur ne kadar güvenli hakkında sorular – Microsoft için kendi duyarlı iş verilerinin kullanıyor mu?</span><span class="sxs-lookup"><span data-stu-id="e687c-187">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="e687c-188">Ve yanıt Evet biz yapın.</span><span class="sxs-lookup"><span data-stu-id="e687c-188">And the answer is yes we do.</span></span> <span data-ttu-id="e687c-189">Örneğin iç Microsoft SharePoint sitesinde gidin, [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), oturum açmak için girmem isteniyor.</span><span class="sxs-lookup"><span data-stu-id="e687c-189">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Office 365 oturum açma](single-sign-on/_static/image20.png)

<span data-ttu-id="e687c-191">Microsoft, ADFS, etkinleştirilmiş şekilde a Microsoft ID girdiğinizde, ADFS oturum açma sayfasına yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e687c-191">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![ADFS oturum açma](single-sign-on/_static/image21.png)

<span data-ttu-id="e687c-193">Ve bir iç Microsoft AD hesapta depolanan kimlik bilgileri girdikten sonra bu dahili uygulama erişimi.</span><span class="sxs-lookup"><span data-stu-id="e687c-193">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![MS SharePoint sitesi](single-sign-on/_static/image22.png)

<span data-ttu-id="e687c-195">Çoğunlukla biz Azure AD kullanılabilir hale geldi, ancak oturum açma işleminiz bulutta Azure AD dizini üzerinden geçmeden önce ayarlamanız ADFS zaten biz oturum açma bir AD sunucusu kullanıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="e687c-195">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="e687c-196">Biz bizim önemli belgeleri, kaynak denetimi, performans Yönetim dosyaları, satış raporları ve daha fazla bulutta koyun ve bunları güvenli hale getirmek için bu tam aynı çözümü kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="e687c-196">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="e687c-197">Azure AD çoklu oturum açma için kullandığı bir ASP.NET uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e687c-197">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="e687c-198">Visual Studio birkaç ekran görüntüleri gördüğünüz Azure AD çoklu oturum açma için kullanan bir uygulama oluşturmak gerçekten kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="e687c-198">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="e687c-199">Yeni bir ASP.NET uygulaması, MVC veya Web Forms oluşturduğunuzda, varsayılan kimlik doğrulama yöntemi ASP.NET kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="e687c-199">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="e687c-200">Azure AD ile değiştirmek için tıklattığınız bir **değiştirmek kimlik doğrulama** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e687c-200">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![Kimlik doğrulamayı Değiştir](single-sign-on/_static/image23.png)

<span data-ttu-id="e687c-202">Kuruluş hesabı seçin, etki alanı adınızı girin ve ardından çoklu oturum açma seçin.</span><span class="sxs-lookup"><span data-stu-id="e687c-202">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![Kimlik doğrulama iletişim yapılandırın](single-sign-on/_static/image24.png)

<span data-ttu-id="e687c-204">Ayrıca uygulama okuma veya okuma/yazma izni dizin veriler için.</span><span class="sxs-lookup"><span data-stu-id="e687c-204">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="e687c-205">Bunu yaparsanız, kullanabilirsiniz [Azure grafik REST API'sini](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) kullanıcıların telefon numarasını aramak için son oturum zaman üzerinde vb. ofiste oldukları varsa öğrenin.</span><span class="sxs-lookup"><span data-stu-id="e687c-205">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="e687c-206">Tüm yapmanız gereken - olan Visual Studio için kimlik bilgileri Azure AD kiracınız için yönetici ister ve ardından projenizi ve Azure AD kiracınıza yeni uygulama için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e687c-206">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="e687c-207">Projeyi çalıştırdığınızda, bir oturum açma sayfasını görürsünüz ve Azure AD dizininde bir kullanıcı kimlik bilgileriyle oturum açabilirler.</span><span class="sxs-lookup"><span data-stu-id="e687c-207">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![Kurumsal hesap oturum açma](single-sign-on/_static/image25.png)

![Oturum açanlar](single-sign-on/_static/image26.png)

<span data-ttu-id="e687c-210">Azure için uygulama dağıttığınızda, yapmanız gereken tek şey seçin bir **kuruluş kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin ve bir kez daha Visual Studio sizin için tüm yapılandırma ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="e687c-210">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![Web yayımlama](single-sign-on/_static/image27.png)

<span data-ttu-id="e687c-212">Bu ekran görüntüleri Azure AD kimlik doğrulaması kullanan bir uygulama oluşturmak nasıl oluşturulduğunu gösteren tam bir adım adım öğretici gelir: [Azure Active Directory ile ASP.NET uygulamaları geliştirmek](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="e687c-212">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="e687c-213">Özet</span><span class="sxs-lookup"><span data-stu-id="e687c-213">Summary</span></span>

<span data-ttu-id="e687c-214">Bu bölümde Azure Active Directory, Visual Studio ve ASP.NET, kuruluşunuzdaki kullanıcılar için Internet uygulamalarında Çoklu oturum açma ayarlamak kolaylaştırır, gördünüz.</span><span class="sxs-lookup"><span data-stu-id="e687c-214">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="e687c-215">Kullanıcılarınızın Internet uygulamalarda iç ağınızda Active Directory kullanarak oturum için kullandıkları aynı kimlik bilgilerini kullanarak oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="e687c-215">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="e687c-216">[Sonraki bölümde](data-storage-options.md) bulut uygulaması için kullanılabilir veri depolama seçenekleri bakar.</span><span class="sxs-lookup"><span data-stu-id="e687c-216">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="e687c-217">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e687c-217">Resources</span></span>

<span data-ttu-id="e687c-218">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="e687c-218">For more information, see the following resources:</span></span>

- <span data-ttu-id="e687c-219">[Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="e687c-219">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="e687c-220">Azure AD belgelerinde windowsazure.com site için portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="e687c-220">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="e687c-221">Adım adım öğreticiler için bkz: **geliştirme** bölümü.</span><span class="sxs-lookup"><span data-stu-id="e687c-221">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="e687c-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span><span class="sxs-lookup"><span data-stu-id="e687c-222">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="e687c-223">Azure çok faktörlü kimlik doğrulaması ile ilgili belgeler için portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="e687c-223">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="e687c-224">[Kurumsal hesap kimlik doğrulama seçeneklerini](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span><span class="sxs-lookup"><span data-stu-id="e687c-224">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="e687c-225">Visual Studio 2013 yeni proje iletişim kutusunda Azure AD kimlik doğrulama seçenekleri açıklaması.</span><span class="sxs-lookup"><span data-stu-id="e687c-225">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="e687c-226">[Microsoft Patterns and Practices - Federal Kimlik düzeni](https://msdn.microsoft.com/library/dn589790.aspx).</span><span class="sxs-lookup"><span data-stu-id="e687c-226">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="e687c-227">[Nasıl yapılır: Azure Active Directory eşitleme aracını yüklemek](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span><span class="sxs-lookup"><span data-stu-id="e687c-227">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="e687c-228">[Active Directory Federasyon Hizmetleri 2.0 içerik haritası](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span><span class="sxs-lookup"><span data-stu-id="e687c-228">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="e687c-229">ADFS 2.0 hakkındaki belgelere bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="e687c-229">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="e687c-230">[Windows Azure AD uygulama rolü ve ACL tabanlı yetkilendirme](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span><span class="sxs-lookup"><span data-stu-id="e687c-230">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="e687c-231">Örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="e687c-231">Sample application.</span></span>
- <span data-ttu-id="e687c-232">[Azure Active Directory grafik API'si blogu](https://blogs.msdn.com/b/aadgraphteam/).</span><span class="sxs-lookup"><span data-stu-id="e687c-232">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="e687c-233">[Erişim denetimi KCG ve karma kimlik altyapınızı dizin tümleştirme](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span><span class="sxs-lookup"><span data-stu-id="e687c-233">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="e687c-234">Teknik Ed 2014 oturumunda video Gayana Bagdasaryan.</span><span class="sxs-lookup"><span data-stu-id="e687c-234">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e687c-235">[Önceki](web-development-best-practices.md)
[sonraki](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="e687c-235">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
