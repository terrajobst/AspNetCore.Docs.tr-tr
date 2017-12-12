# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="11393-101">Nasıl yapı/güvenli kullanıcı veri örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="11393-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="11393-102">Parola Yöneticisi Aracı ile parola ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="11393-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="11393-103">Veritabanını güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="11393-103">Update the database:</span></span>

    `dotnet ef database update`
