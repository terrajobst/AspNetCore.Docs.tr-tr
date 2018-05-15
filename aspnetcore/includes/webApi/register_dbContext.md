## <a name="register-the-database-context"></a><span data-ttu-id="09dec-101">Veritabanı bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="09dec-101">Register the database context</span></span>

<span data-ttu-id="09dec-102">Bu adımda, veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="09dec-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="09dec-103">Bağımlılık ekleme (dı) kapsayıcı ile kaydedilen Hizmetleri (örneğin, DB bağlamı) denetleyicileri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="09dec-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="09dec-104">DB bağlamı için yerleşik destek kullanarak hizmet kapsayıcısı kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="09dec-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="09dec-105">Değiştir *haline* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="09dec-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="09dec-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="09dec-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="09dec-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="09dec-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>
::: moniker-end

<span data-ttu-id="09dec-108">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="09dec-108">The preceding code:</span></span>

* <span data-ttu-id="09dec-109">Kullanılmayan kodunu kaldırır.</span><span class="sxs-lookup"><span data-stu-id="09dec-109">Removes the unused code.</span></span>
* <span data-ttu-id="09dec-110">Hizmet kapsayıcıya bir bellek içi veritabanına eklenen belirtir.</span><span class="sxs-lookup"><span data-stu-id="09dec-110">Specifies an in-memory database is injected into the service container.</span></span>