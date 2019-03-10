---
author: pkrebs
ms.author: pkrebs
title: Actualización de aprendizaje personalizada
ms.date: 02/10/2019
description: Aprendizaje personalizado de la configuración manual del elemento Web de Office 365
ms.openlocfilehash: f9729c922b374cc6b775737fa7c7c76a4719534c
ms.sourcegitcommit: b6617bbbaee0784d6216e96052c2469f97cf51e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "30411900"
---
# <a name="manual-upgrade-for-custom-learning"></a><span data-ttu-id="2b9e1-103">Actualización manual de aprendizaje personalizado</span><span class="sxs-lookup"><span data-stu-id="2b9e1-103">Manual Upgrade for Custom Learning</span></span>

<span data-ttu-id="2b9e1-104">Aprendizaje personalizado para Office 365 proporciona un proceso de actualización manual para organizaciones que participaron en pilotos anteriores.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-104">Custom Learning for Office 365 provides a manual upgrade process for organizations that have participated in earlier pilots.</span></span> <span data-ttu-id="2b9e1-105">Con el proceso de actualización, las organizaciones pueden seguir usando su sitio de aprendizaje personalizado actual y actualizarlo agregando el nuevo y mejorado elemento Web de aprendizaje personalizado a su catálogo de aplicaciones de SharePoint y, a continuación, ejecutando un script de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-105">With the upgrade process, organizations can continue to use their current Custom Learning site and upgrade by adding the new, enhanced Custom Learning web part to their SharePoint App Catalog, and then running a PowerShell script.</span></span> <span data-ttu-id="2b9e1-106">A continuación, se proporciona información general sobre el proceso de actualización:</span><span class="sxs-lookup"><span data-stu-id="2b9e1-106">The following provides an overview of the upgrade process:</span></span> 

- <span data-ttu-id="2b9e1-107">Validar que la persona responsable de cargar el nuevo elemento Web y que ejecuta el script de PowerShell tiene los permisos necesarios</span><span class="sxs-lookup"><span data-stu-id="2b9e1-107">Validate that the person responsible for uploading the new web part and running the Powershell script has the required permissions</span></span>
- <span data-ttu-id="2b9e1-108">Cargar el elemento Web, el archivo customlearning. sppkg, en el catálogo de aplicaciones del espacio empresarial de Office 365</span><span class="sxs-lookup"><span data-stu-id="2b9e1-108">Upload the web part, customlearning.sppkg file, to your Office 365 Tenant App Catalog</span></span>
- <span data-ttu-id="2b9e1-109">Ejecutar un script de PowerShell que configurará el inquilino con los artefactos adecuados necesarios para el aprendizaje personalizado</span><span class="sxs-lookup"><span data-stu-id="2b9e1-109">Execute a PowerShell script that will configure your tenant with the appropriate artifacts required for Custom Learning</span></span>
- <span data-ttu-id="2b9e1-110">Navegue a la página CustomLearningAdmin. aspx en el sitio de aprendizaje personalizado para inicializar la configuración de CContent personalizada.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-110">Navigate to the CustomLearningAdmin.aspx page in the Custom Learning site to to initialize the Custom Ccontent configuration.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b9e1-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2b9e1-111">Prerequisites</span></span>
<span data-ttu-id="2b9e1-112">Para garantizar una actualización correcta del aprendizaje personalizado, se deben cumplir los siguientes requisitos previos.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-112">To ensure a successful upgrade of Custom Learning, the following prerequisites must be met.</span></span> 

- <span data-ttu-id="2b9e1-113">Debe tener configurado un catálogo de aplicaciones para todos los inquilinos.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-113">You must have set up a tenant-wide App Catalog.</span></span> <span data-ttu-id="2b9e1-114">Si no tiene un catálogo de aplicaciones del espacio empresarial, consulte [configurar el inquilino de Office 365](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant#create-app-catalog-site) y siga la sección crear un sitio del catálogo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-114">If you don't have a Tenant App Catalog, please see [Set up your Office 365 tenant](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/set-up-your-developer-tenant#create-app-catalog-site) and follow the Create app catalog site section.</span></span> 
- <span data-ttu-id="2b9e1-115">Si ya se ha aprovisionado el catálogo de aplicaciones de todo el espacio empresarial, necesita tener acceso a una cuenta que tenga derechos para cargar un paquete en él.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-115">If your tenant-wide App Catalog has already been provisioned, you need access to an account that has rights to upload a package to it.</span></span> <span data-ttu-id="2b9e1-116">Por lo general, se trata de una cuenta con el rol de administrador de SharePoint.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-116">Generally this is an account with the SharePoint Administrator role.</span></span> 
- <span data-ttu-id="2b9e1-117">Si una cuenta con el rol de administrador de SharePoint no funciona: vaya al centro de administración de SharePoint, seleccione el catálogo de aplicaciones, haga clic en propietarios e inicie sesión como uno de los administradores de la colección de sitios o agregue la cuenta de administrador de SharePoint que produjo el error en el sitio. Lista de administradores de la colección.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-117">If an account with the SharePoint Administrator's role doesn't work: Go to the SharePoint Admin Center, select the App Catalog, click Owners, and sign in as one of the Site Collection Administrators or add the SharePoint Administrator account that failed to the Site Collection Administrators list.</span></span> 

## <a name="step-1---get-the-web-part-package-and-setup-script-from-github"></a><span data-ttu-id="2b9e1-118">Paso 1: obtener el paquete de elementos Web y la secuencia de comandos de instalación de GitHub</span><span class="sxs-lookup"><span data-stu-id="2b9e1-118">Step 1 - Get the web part package and setup script from GitHub</span></span>
<span data-ttu-id="2b9e1-119">Como parte del proceso de instalación, necesitará el paquete de elementos Web de aprendizaje personalizado y el script de configuración de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-119">As part of the setup process, you'll need the Custom Learning Web part package and the PowerShell Setup Script.</span></span>

1. <span data-ttu-id="2b9e1-120">Vaya al [repositorio de github de aprendizaje personalizado](https://github.com/pnp/custom-learning-office-365).</span><span class="sxs-lookup"><span data-stu-id="2b9e1-120">Go the the [Custom Learning GitHub Repository](https://github.com/pnp/custom-learning-office-365).</span></span>
2. <span data-ttu-id="2b9e1-121">Haga clic en **clonar o descargar**y, a continuación, **Descargue zip**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-121">Click **Clone or download**, and then **Download ZIP**.</span></span>   
3. <span data-ttu-id="2b9e1-122">Guarde el archivo **zip** en una ubicación de la unidad local.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-122">Save the **ZIP** file to a location on your local drive.</span></span>
4. <span data-ttu-id="2b9e1-123">Extraiga el archivo **zip** en la unidad local.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-123">Extract the **ZIP** file on your local drive.</span></span>

## <a name="step-2---upload-the-web-part-to-the-tenant-app-catalog"></a><span data-ttu-id="2b9e1-124">Paso 2: cargar el elemento Web en el catálogo de aplicaciones del espacio empresarial</span><span class="sxs-lookup"><span data-stu-id="2b9e1-124">Step 2 - Upload the web part to the Tenant App Catalog</span></span>
<span data-ttu-id="2b9e1-125">Para configurar el aprendizaje personalizado para Office 365, debe cargar el archivo customlearning. sppkg en el catálogo de aplicaciones de todo el inquilino e implementarlo.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-125">To set up Custom Learning for Office 365, you upload the customlearning.sppkg file to the tenant-wide App Catalog and deploy it.</span></span> <span data-ttu-id="2b9e1-126">Para obtener instrucciones detalladas sobre cómo agregar una aplicación al catálogo de aplicaciones, vea [usar el catálogo de aplicaciones para hacer que las aplicaciones empresariales personalizadas estén disponibles en su entorno de SharePoint Online](https://docs.microsoft.com/en-us/sharepoint/use-app-catalog) .</span><span class="sxs-lookup"><span data-stu-id="2b9e1-126">Please see [Use the App Catalog to make custom business apps available for your SharePoint Online environment](https://docs.microsoft.com/en-us/sharepoint/use-app-catalog) for detailed instructions on how to add an app to the app catalog.</span></span>

1. <span data-ttu-id="2b9e1-127">En Office 365, haga clic en **Administrador**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-127">In Office 365, click **Admin**.</span></span>
2. <span data-ttu-id="2b9e1-128">En **centros de administración**, haga clic en **SharePoint**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-128">Under **Admin centers**, click **SharePoint**.</span></span>
3. <span data-ttu-id="2b9e1-129">Haga clic en **aplicaciones** > **Catálogo** > **de aplicaciones distribute apps for SharePoint.**</span><span class="sxs-lookup"><span data-stu-id="2b9e1-129">Click **apps** > **App Catalog** > **Distribute apps for SharePoint.**</span></span>
4. <span data-ttu-id="2b9e1-130">Haga clic en **cargar** > **archivos de selección**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-130">Click **Upload** > **Choose Files**.</span></span>
5. <span data-ttu-id="2b9e1-131">En la carpeta en la que guardó el archivo ZIP, seleccione \*\*\*\* la carpeta WebPart y, a continuación, seleccione **customlearning. sppkg.**</span><span class="sxs-lookup"><span data-stu-id="2b9e1-131">In the folder where you saved the ZIP file, select the **webpart** folder, and then select **customlearning.sppkg.**</span></span>
6. <span data-ttu-id="2b9e1-132">Haga clic en **Implementar**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-132">Click **Deploy**.</span></span>

## <a name="step-5--execute-powershell-configuration-script"></a><span data-ttu-id="2b9e1-133">Paso 5: ejecutar el script de configuración de PowerShell</span><span class="sxs-lookup"><span data-stu-id="2b9e1-133">Step 5- Execute PowerShell Configuration Script</span></span>
<span data-ttu-id="2b9e1-134">Un script `CustomLearningConfiguration.ps1` de PowerShell se incluye en la descarga de zip desde github.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-134">A PowerShell script `CustomLearningConfiguration.ps1` is included in the ZIP download from GitHub.</span></span> <span data-ttu-id="2b9e1-135">Debe ejecutar el script para crear tres propiedades de [inquilino](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/tenant-properties) que usa la solución.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-135">You need to execute the script to create three [tenant properties](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/tenant-properties) that the solution uses.</span></span> <span data-ttu-id="2b9e1-136">Además, el script crea dos [páginas de aplicación de elemento único](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/single-part-app-pages) en la biblioteca de páginas del sitio para hospedar los elementos Web de administrador y de usuario en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-136">In addition, the script creates two [single part app pages](https://docs.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/single-part-app-pages) in the site pages library to host the admin and user web parts at a known location.</span></span> <span data-ttu-id="2b9e1-137">Estas son las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2b9e1-137">These app pages are:</span></span>

- <span data-ttu-id="2b9e1-138">CustomAdministration. aspx</span><span class="sxs-lookup"><span data-stu-id="2b9e1-138">CustomAdministration.aspx</span></span>
- <span data-ttu-id="2b9e1-139">CustomViewer. aspx</span><span class="sxs-lookup"><span data-stu-id="2b9e1-139">CustomViewer.aspx</span></span>

### <a name="to-run-the-powershell-script"></a><span data-ttu-id="2b9e1-140">Para ejecutar el script de PowerShell</span><span class="sxs-lookup"><span data-stu-id="2b9e1-140">To run the Powershell Script</span></span>
- <span data-ttu-id="2b9e1-141">Con PowerShell, ejecute el `CustomLearningConfiguration.ps1` script que se encuentra en la carpeta WebPart desde el archivo zip de github.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-141">Using PowerShell, execute the `CustomLearningConfiguration.ps1` script located in the webpart folder from the GitHub ZIP.</span></span> <span data-ttu-id="2b9e1-142">Si se ejecuta correctamente, verá tres pares clave-valor y **Administrador de aprendizaje personalizado para desactivado...** en la ventana de comandos.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-142">If successful, you'll see three key/value pairs and **Custom Learning Admin for Off...** in the Command window.</span></span>

### <a name="disabling-telemetry-collection"></a><span data-ttu-id="2b9e1-143">DesHabilitar la colección de telemetría</span><span class="sxs-lookup"><span data-stu-id="2b9e1-143">Disabling Telemetry Collection</span></span>
<span data-ttu-id="2b9e1-144">Aprendizaje personalizado incluye el seguimiento de telemetría de anonimizan, que de forma predeterminada se establece en activado.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-144">Custom Learning includes anonymized telemetry tracking opt in, which by default is set to on.</span></span> <span data-ttu-id="2b9e1-145">Si desea desactivar el seguimiento de telemetría, cambie el `CustomlearningConfiguration.ps1` script para establecer la `$optInTelemetry` variable en. `$false`</span><span class="sxs-lookup"><span data-stu-id="2b9e1-145">If you  would like to turn telemetry tracking off, please change the `CustomlearningConfiguration.ps1` script to set the `$optInTelemetry` variable to `$false`.</span></span>

## <a name="step-6---initialize-web-part-custom-configuration"></a><span data-ttu-id="2b9e1-146">Paso 6: inicializar la configuración personalizada del elemento Web</span><span class="sxs-lookup"><span data-stu-id="2b9e1-146">Step 6 - Initialize web part custom configuration</span></span>
<span data-ttu-id="2b9e1-147">Una vez ejecutado correctamente el script de PowerShell, navegue `<YOUR-SITE-COLLECTION-URL>/SitePages/CustomLearningAdmin.aspx`a.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-147">After the PowerShell script is successfully run, navigate to `<YOUR-SITE-COLLECTION-URL>/SitePages/CustomLearningAdmin.aspx`.</span></span> <span data-ttu-id="2b9e1-148">Al abrir **CustomLearningAdmin. aspx** se inicializa el elemento de lista **CustomConfig** que configura el aprendizaje personalizado para el primer uso.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-148">Opening **CustomLearningAdmin.aspx** initializes the **CustomConfig** list item that sets up Custom Learning for first use.</span></span> <span data-ttu-id="2b9e1-149">Debería ver una página similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="2b9e1-149">You should see a page that looks like this:</span></span>

![CG-adminapppage. png](media/cg-adminapppage.png)

## <a name="add-owners-to-site"></a><span data-ttu-id="2b9e1-151">Agregar propietarios al sitio</span><span class="sxs-lookup"><span data-stu-id="2b9e1-151">Add Owners to Site</span></span>
<span data-ttu-id="2b9e1-152">Como administrador de inquilinos, es poco probable que sea la persona que va a personalizar el sitio, por lo que necesitará asignar algunos propietarios al sitio.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-152">As the Tenant Admin, it's unlikely you'll be the person customizing the site, so you'll need to assign a few owners to the site.</span></span> <span data-ttu-id="2b9e1-153">Los propietarios tienen privilegios administrativos en el sitio para que puedan modificar las páginas del sitio y remarcar el sitio.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-153">Owners have administrative privileges on the site so they can modify site pages and rebrand the site.</span></span> <span data-ttu-id="2b9e1-154">También tienen la posibilidad de ocultar y mostrar contenido entregado a través del elemento Web de aprendizaje personalizado.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-154">They also have the ability to hide and show content delivered through the Custom Learning Web part.</span></span> <span data-ttu-id="2b9e1-155">También tendrán la capacidad de crear una lista de reproducción personalizada y asignarlas a subcategorías personalizadas.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-155">They'll also have the ability to build custom playlist and assign them to custom subcategories.</span></span>  

1. <span data-ttu-id="2b9e1-156">En el menú **configuración** de SharePoint, haga clic en **permisos del sitio**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-156">From the SharePoint **Settings** menu, click **Site Permissions**.</span></span>
2. <span data-ttu-id="2b9e1-157">Haga clic en **Configuración avanzada de permisos**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-157">Click **Advanced Permission Settings**.</span></span>
3. <span data-ttu-id="2b9e1-158">Haga clic en **aprendizaje personalizado para los propietarios de Office 365**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-158">Click **Custom learning for Office 365 Owners**.</span></span>
4. <span data-ttu-id="2b9e1-159">Haga clic en **nuevo** > **Agregar usuarios a este grupo**, agregue a los usuarios que desea que sean propietarios y, a continuación, haga clic en **compartir**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-159">Click **New** > **Add Users to this group**, add the people you want to be Owners, and then click **Share**.</span></span>

<span data-ttu-id="2b9e1-160">La actualización ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-160">The upgrade is now complete.</span></span> <span data-ttu-id="2b9e1-161">Para obtener más información sobre cómo adaptar el sitio de aprendizaje y el elemento web personalizados para su entorno, vea [customize the Training Experience](custom_overview.md).</span><span class="sxs-lookup"><span data-stu-id="2b9e1-161">To learn more about how to tailor the Custom Learning site and web part for your environment, see [Customize the training experience](custom_overview.md).</span></span>

## <a name="upgrade-instructions-for-site-owners"></a><span data-ttu-id="2b9e1-162">Instrucciones de actualización para los propietarios del sitio</span><span class="sxs-lookup"><span data-stu-id="2b9e1-162">Upgrade Instructions for Site Owners</span></span>
<span data-ttu-id="2b9e1-163">Aprendizaje personalizado para Office 365 ha incorporado una amplia variedad de mejoras en el elemento Web.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-163">Custom Learning for office 365 has introduced a variety of enhancements to the web part.</span></span> <span data-ttu-id="2b9e1-164">El trabajo con el elemento Web se describe con detalle en la sección [personalizar la experiencia de aprendizaje](custom_overview.md) .</span><span class="sxs-lookup"><span data-stu-id="2b9e1-164">Working with the web part is covered in detail in the [Customize the learning experience](custom_overview.md) section.</span></span> <span data-ttu-id="2b9e1-165">Por ahora, el propietario del sitio debe:</span><span class="sxs-lookup"><span data-stu-id="2b9e1-165">For now, the Site Owner should:</span></span>  

- <span data-ttu-id="2b9e1-166">Compruebe que el elemento Web de aprendizaje personalizado para Office 365 está disponible.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-166">Verify the Custom Learning for Office 365 Web part is available.</span></span> 
- <span data-ttu-id="2b9e1-167">Reemplazar el elemento Web existente en páginas con el nuevo elemento Web</span><span class="sxs-lookup"><span data-stu-id="2b9e1-167">Replace the existing web part on pages with the new web part</span></span>
- <span data-ttu-id="2b9e1-168">Reemplazar los vínculos que apuntan al elemento web antiguo</span><span class="sxs-lookup"><span data-stu-id="2b9e1-168">Replace any links pointing to the old web part</span></span>

### <a name="verify-the-custom-learning-for-office-365-web-part-is-available"></a><span data-ttu-id="2b9e1-169">Comprobar que el elemento Web de aprendizaje personalizado para Office 365 está disponible</span><span class="sxs-lookup"><span data-stu-id="2b9e1-169">Verify the Custom Learning for Office 365 web part is available</span></span>
1.  <span data-ttu-id="2b9e1-170">En el sitio de aprendizaje personalizado, haga clic en **configuración**y, a continuación, haga clic en \***Agregar una página**.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-170">From the Custom Learning site, click **Settings**, and then click \***Add a Page**.</span></span>
2.  <span data-ttu-id="2b9e1-171">Haga clic **+** en el icono de la página para agregar un elemento Web y, a continuación, seleccione el elemento Web **de aprendizaje personalizado para Office 365** .</span><span class="sxs-lookup"><span data-stu-id="2b9e1-171">Click the **+** icon on the page to add a web part, and then select the **Custom Learning for Office 365** web part.</span></span> <span data-ttu-id="2b9e1-172">La página debería tener ahora un aspecto similar al del siguiente gráfico:</span><span class="sxs-lookup"><span data-stu-id="2b9e1-172">The page should now look similar to the following graphic:</span></span>

[<span data-ttu-id="2b9e1-173">CG-adminapppage. png</span><span class="sxs-lookup"><span data-stu-id="2b9e1-173">cg-adminapppage.png</span></span>](media/cg-adminapppage.png)
 
### <a name="replace-the-old-web-part-with-the-new-web-part"></a><span data-ttu-id="2b9e1-174">Reemplazar el elemento web antiguo con el nuevo elemento Web</span><span class="sxs-lookup"><span data-stu-id="2b9e1-174">Replace the old web part with the new web part</span></span>
<span data-ttu-id="2b9e1-175">Antes de reemplazar el elemento Web de aprendizaje personalizado o realizar cambios en el sitio, le recomendamos que lea la documentación [personalizar la experiencia de aprendizaje](custom_overview.md) , ya que explica cómo usar el nuevo elemento Web.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-175">Before replacing the Custom Learning web part or making changes to the site, we recommend you read the [Customize the Learning Experience](custom_overview.md) documentation as it explains how to use the new web part.</span></span> 

<span data-ttu-id="2b9e1-176">Para actualizar el sitio de aprendizaje personalizado, reemplace las instancias existentes del elemento Web con el nuevo elemento Web.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-176">To upgrade the Custom Learning site, replace existing instances of the Web part with the new Web part.</span></span> <span data-ttu-id="2b9e1-177">Aunque no podemos estar seguro de dónde se ha agregado el elemento Web, la guía para pilotos anteriores era agregar el elemento Web a las siguientes páginas, por lo que busque reemplazar el elemento Web en estas páginas:</span><span class="sxs-lookup"><span data-stu-id="2b9e1-177">While we can’t be sure where the Web part has been added, the guidance for previous pilots was to add the web part to the following pages, so look to replace the web part on these pages:</span></span>

- <span data-ttu-id="2b9e1-178">Get-started-with-Office-365. aspx</span><span class="sxs-lookup"><span data-stu-id="2b9e1-178">Get-started-with-Office-365.aspx</span></span>
- <span data-ttu-id="2b9e1-179">Get-started-with-Microsoft-Teams. aspx</span><span class="sxs-lookup"><span data-stu-id="2b9e1-179">Get-started-with-Microsoft-Teams.aspx</span></span>
- <span data-ttu-id="2b9e1-180">Get-started-with-OneDrive. aspx</span><span class="sxs-lookup"><span data-stu-id="2b9e1-180">Get-started-with-OneDrive.aspx</span></span>
- <span data-ttu-id="2b9e1-181">Get-started-with-SharePoint. aspx</span><span class="sxs-lookup"><span data-stu-id="2b9e1-181">Get-started-with-SharePoint.aspx</span></span>

### <a name="replace-existing-links-to-the-web-part"></a><span data-ttu-id="2b9e1-182">Reemplazar los vínculos existentes al elemento Web</span><span class="sxs-lookup"><span data-stu-id="2b9e1-182">Replace existing links to the web part</span></span>
<span data-ttu-id="2b9e1-183">Con las mejoras del nuevo elemento Web, se ha cambiado la forma en que se vincula a una lista de reproducción.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-183">With the enhancements to the new web part, they way that you link to a playlist has changed.</span></span> <span data-ttu-id="2b9e1-184">Como parte de la actualización, debe reemplazar los vínculos al elemento Web, incluidos los elementos de menú, y los vínculos de la Página principal.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-184">As part of the upgrade, you should replace any links to the web part, including menu items, and links on the Home page.</span></span> <span data-ttu-id="2b9e1-185">Antes de reemplazar el elemento Web de aprendizaje personalizado o realizar cambios en el sitio, le recomendamos que lea la documentación [personalizar la experiencia de aprendizaje](custom_overview.md) , ya que explica cómo usar el nuevo elemento Web.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-185">Before replacing the Custom Learning web part or making changes to the site, we recommend you read the [Customize the Learning Experience](custom_overview.md) documentation as it explains how to use the new web part.</span></span> 

## <a name="recreate-existing-playlists"></a><span data-ttu-id="2b9e1-186">Volver a crear listas de reproducción existentes</span><span class="sxs-lookup"><span data-stu-id="2b9e1-186">Recreate existing playlists</span></span> 
<span data-ttu-id="2b9e1-187">Para asegurarse de que las listas de reproducción funcionan correctamente, se deben volver a crear todas las listas de reproducción que se hayan creado con la versión anterior del elemento Web.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-187">To ensure that playlists work properly, any playlists that have been created with the earlier version of the web part will need to be recreated.</span></span> <span data-ttu-id="2b9e1-188">Antes de eliminar las listas de reproducción, cree una lista de las listas de reproducción personalizadas y los activos asociados para poder volver a crearlas fácilmente con el nuevo elemento Web de aprendizaje personalizado.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-188">Before deleting the playlists, make a list of the custom playlists and associated assets so you can recreate them easily with the new Custom Learning Web part.</span></span> <span data-ttu-id="2b9e1-189">Haga una copia de una lista de reproducción y, a continuación, elimínela.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-189">Make a copy of a playlist and then delete it.</span></span> <span data-ttu-id="2b9e1-190">Puede usar el campo JSONData para realizar una copia del contenido de una lista de reproducción antes de eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-190">You can use the JSONData field to make a copy of the content of a playlist before you delete.</span></span> <span data-ttu-id="2b9e1-191">Esto hará que sea más fácil crear más adelante.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-191">This will make it easier to create later.</span></span>


<span data-ttu-id="2b9e1-192">• En el sitio de aprendizaje personalizado, haga clic en configuración > contenido del sitio.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-192">•   From the Custom Learning site, click Settings > Site Content.</span></span> <span data-ttu-id="2b9e1-193">• Seleccione una lista de reproducción, seleccione los puntos suspensivos, seleccione Editar y, a continuación, copie el contenido del campo JSONData y guárdelo en el Bloc de notas o en un documento independiente para consultarlo más adelante.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-193">•   Select a playlist, select the ellipses, select Edit, and then copy the contents of the JSONData field and save in NotePad or a separate document for later reference.</span></span> <span data-ttu-id="2b9e1-194">Seleccione Cancelar.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-194">Select Cancel.</span></span>
<span data-ttu-id="2b9e1-195">• Seleccione la lista de reproducción, seleccione los puntos suspensivos y, después, seleccione eliminar.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-195">•   Select the playlist, select the ellipses, and then select Delete.</span></span>
<span data-ttu-id="2b9e1-196">• Ya está listo para volver a crear la lista de reproducción con el nuevo elemento Web.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-196">•   You are now ready to recreate the playlist with the new Web part.</span></span>
<span data-ttu-id="2b9e1-197">Para obtener instrucciones sobre cómo usar el nuevo elemento Web de aprendizaje personalizado de Office https://docs.microsoft.com/en-us/office365/customlearning/custom_overview365, consulte.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-197">For instructions on using the new Custom Learning for Office 365 Web part, see https://docs.microsoft.com/en-us/office365/customlearning/custom_overview.</span></span>

## <a name="step-8---chan"></a><span data-ttu-id="2b9e1-198">Paso 8: Chan</span><span class="sxs-lookup"><span data-stu-id="2b9e1-198">Step 8 - Chan</span></span>

### <a name="next-steps"></a><span data-ttu-id="2b9e1-199">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2b9e1-199">Next Steps</span></span>
- <span data-ttu-id="2b9e1-200">[Personalice](custom_overview.md) la experiencia de aprendizaje para su organización.</span><span class="sxs-lookup"><span data-stu-id="2b9e1-200">[Customize](custom_overview.md) the training experience for your organization.</span></span>
