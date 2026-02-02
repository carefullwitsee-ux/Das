# Importar un repositorio con el Importador GitHub

Si tiene un proyecto hospedado en otro sistema de control de versiones, puede importarlo automáticamente a GitHub mediante la herramienta GitHub Importer.

## Acerca de las migraciones de repositorios con GitHub Importer

GitHub Importer importa el código fuente y el historial de confirmaciones de repositorios de Git hospedados en servicios de hospedaje externos. Para más información sobre las funcionalidades y limitaciones de GitHub Pages, consulta [Acerca de Importador GitHub](/es/migrations/importing-source-code/using-github-importer/about-github-importer#capabilities-and-limitations-of-github-importer).

GitHub usa la dirección de correo electrónico en el encabezado de la confirmación para vincular la confirmación con un usuario de GitHub. Para asignar correctamente confirmaciones en un repositorio importado, los usuarios deberán agregar la dirección de correo electrónico asociada a sus confirmaciones a su cuenta de GitHub. Para más información, consulta [Agregar una dirección de correo electrónico a tu cuenta de GitHub](/es/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/adding-an-email-address-to-your-github-account).

## Importación de un repositorio con GitHub Importer

Al importar un repositorio con GitHub Importer, se creará un nuevo repositorio. Si ya tiene un repositorio existente que desea usar, puede agregar el repositorio local a GitHub mediante Git. Para más información, consulta [Agregar código hospedado localmente a GitHub](/es/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github#importing-a-git-repository-with-the-command-line).

1. En la esquina superior derecha de cualquier página de GitHub.com, haz clic en <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-plus" aria-label="Create new" role="img"><path d="M7.75 2a.75.75 0 0 1 .75.75V7h4.25a.75.75 0 0 1 0 1.5H8.5v4.25a.75.75 0 0 1-1.5 0V8.5H2.75a.75.75 0 0 1 0-1.5H7V2.75A.75.75 0 0 1 7.75 2Z"></path></svg> y, a continuación, haz clic en **Import repository**.

   ![Captura de pantalla de la esquina superior derecha de cualquier página de GitHub. Un icono más está resaltado con un contorno naranja.](/assets/images/help/importer/import-repository.png)

2. En la página “Importar el proyecto a GitHub”, escriba la dirección URL del repositorio remoto hospedado en otra plataforma.

3. Si el repositorio de origen es privado, escriba las credenciales para la autenticación. GitHub Importer usará las credenciales para realizar una operación `git clone` en el repositorio de origen.

4. Elija un propietario y un nombre para el nuevo repositorio en GitHub.

5. Elija la visibilidad del nuevo repositorio. Para más información, consulta [Acerca de los repositorios](/es/repositories/creating-and-managing-repositories/about-repositories#about-repository-visibility).

6. Haga clic en **Comenzar importación**.

Se le redirigirá a una página “Preparación del nuevo repositorio”, donde puede realizar un seguimiento del estado de la importación. Recibirás un correo electrónico cuando se haya importado todo el repositorio.