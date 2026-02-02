# Generación de una nueva clave SSH y adición al agente SSH

Una vez que has comprobado las claves SSH existentes, puedes generar una nueva clave SSH para usarla para la autenticación y luego agregarla al ssh-agent.

## Acerca de las frases de contraseña de clave SSH

Puedes acceder y escribir datos en repositorios de GitHub.com mediante SSH (protocolo Secure Shell). Al conectarse a través de SSH, se realiza la autenticación mediante un archivo de clave privada en el equipo local. Para más información, consulta [Acerca de SSH](/es/authentication/connecting-to-github-with-ssh/about-ssh).

Al generar una clave SSH, puedes agregar una frase de contraseña para proteger aún más la clave. Siempre que uses la clave, debes escribir la frase de contraseña. Si la clave tiene una frase de contraseña y no quieres escribir la frase de contraseña cada vez que uses la clave, puedes agregar la clave al agente SSH. El agente SSH administra las claves SSH y recuerda la frase de contraseña.

Si todavía no tienes una llave SSH, debes generar una nueva para utilizarla para autenticación. Si no estás seguro si ya tienes una llave SSH, puedes verificar si hay llaves existentes. Para más información, consulta [Comprobar tus claves SSH existentes](/es/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys).

Si quieres utilizar una clave de seguridad de hardware para autenticarte en GitHub, debes generar una clave SSH nueva para esta. Debes conectar tu llave de seguridad de hardware a tu computadora cuando te autentiques con el par de llaves. Para obtener más información, consulta las [notas de la versiónde OpenSSH 8.2](https://www.openssh.com/txt/release-8.2).

## Generar una nueva clave SSH

Puedes generar una nueva clave SSH en el equipo local. Después de generar la clave, puede agregar la clave pública a la cuenta en GitHub.com para habilitar la autenticación para las operaciones de Git a través de SSH.

> \[!NOTE]
> En GitHub se mejoró la seguridad mediante la eliminación de los tipos de clave antiguos y no seguros el 15 de marzo de 2022.
>
> A partir de esa fecha, ya no se admiten las claves DSA (`ssh-dss`). No puedes agregar claves DSA nuevas a tu cuenta personal en .
>
> Las claves RSA (`ssh-rsa`) con `valid_after` antes del 2 de noviembre de 2021 pueden seguir usando cualquier algoritmo de firma. Las llaves RSA que se generaron después de esta fecha deberán utilizar un algoritmo de firma de tipo SHA-2. Puede ser que algunos clientes antiguos necesiten actualizar para poder utilizar firmas de tipo SHA-2.

1. Abra <span class="platform-mac">Terminal</span><span class="platform-linux">Terminal</span><span class="platform-windows">Git Bash</span>.

2. Pega el texto siguiente, reemplazando el correo electrónico usado en el ejemplo por tu dirección de correo electrónico de GitHub.

   ```shell
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

   > \[!NOTE]
   > Si usas un sistema heredado que no admite el algoritmo Ed25519, usa:
   >
   > ```shell
   > ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   > ```

   Esto crea una llave SSH utilizando el correo electrónico proporcionado como etiqueta.

   ```shell
   > Generating public/private ALGORITHM key pair.
   ```

   Cuando se te pida: "Introduce un archivo en el que se pueda guardar la clave", teclea **Enter** para aceptar la ubicación de archivo predeterminada. Ten en cuenta que si ya creaste claves SSH anteriormente, ssh-keygen puede pedirte que vuelvas a escribir otra clave. En este caso, se recomienda crear una clave SSH con nombre personalizado. Para ello, escribe la ubicación de archivo predeterminada y reemplaza id\_ALGORITHM por el nombre de clave personalizado.

   <div class="ghd-tool mac">

   ```shell
   > Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]
   ```

   </div>

   <div class="ghd-tool windows">

   ```powershell
   > Enter file in which to save the key (/c/Users/YOU/.ssh/id_ALGORITHM):[Press enter]
   ```

   </div>

   <div class="ghd-tool linux">

   ```shell
   > Enter a file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
   ```

   </div>

3. Cuando se le pida, escriba una frase de contraseña segura. Para más información, consulta [Trabajar con contraseñas de clave SSH](/es/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases).

   ```shell
   > Enter passphrase (empty for no passphrase): [Type a passphrase]
   > Enter same passphrase again: [Type passphrase again]
   ```

## Agregar tu clave SSH al ssh-agent

Antes de agregar una llave SSH nueva al ssh-agent para que administre tus llaves, debes haber verificado si habían llaves SSH existentes y haber generado una llave SSH nueva. <span class="platform-mac">Al agregar la clave SSH al agente, usa el comando `ssh-add` de macOS predeterminado y no una aplicación instalada por [macports](https://www.macports.org/), [homebrew](https://brew.sh/) o algún otro origen externo.</span>

<div class="ghd-tool mac">

1. Inicia el agente SSH en segundo plano.

   ```shell
   $ eval "$(ssh-agent -s)"
   > Agent pid 59566
   ```

   Dependiendo de tu ambiente, puede que necesites utilizar un comando diferente. Por ejemplo, es posible que tenga que usar el acceso raíz mediante la ejecución de `sudo -s -H` antes de iniciar ssh-agent, o bien que tenga que usar `exec ssh-agent bash` o `exec ssh-agent zsh` ejecutar ssh-agent.

2. Si estás usando macOS Sierra 10.12.2 o una versión posterior, deberás modificar tu archivo `~/.ssh/config` para cargar las claves automáticamente en el agente ssh-agent y almacenar las contraseñas en tu cadena de claves.

   * En primer lugar, comprueba si el archivo `~/.ssh/config` existe en la ubicación predeterminada.

     ```shell
     $ open ~/.ssh/config
     > The file /Users/YOU/.ssh/config does not exist.
     ```

   * Si el archivo no existe, créalo.

     ```shell
     touch ~/.ssh/config
     ```

   * Abre el archivo `~/.ssh/config` y, a continuación, modifica el archivo para que contenga las líneas siguientes. Si tu llave SSH tiene un nombre o ruta diferentes que el código de ejemplo, modifica el nombre de archivo o ruta para que coincida con tu configuración actual.

     ```text copy
     Host github.com
       AddKeysToAgent yes
       UseKeychain yes
       IdentityFile ~/.ssh/id_ed25519
     ```

     > \[!NOTE]
     >
     > * Si decides no agregar una frase de contraseña a la clave, debes omitir la línea `UseKeychain`.
     > * Si ves un error `Bad configuration option: usekeychain`, agrega una línea adicional a la sección `Host *.github.com` de la configuración.
     >
     > ```text copy
     > Host github.com
     >   IgnoreUnknown UseKeychain
     > ```

3. Agrega tu llave privada SSH al ssh-agent y almacena tu contraseña en tu keychain. Si has creado tu clave con otro nombre o si vas a agregar una clave existente que tiene otro nombre, reemplaza *id\_ed25519* en el comando por el nombre de tu archivo de clave privada.

   ```shell
   ssh-add --apple-use-keychain ~/.ssh/id_ed25519
   ```

   > \[!NOTE]
   > La opción `--apple-use-keychain` almacena la frase de contraseña en tu cadena de claves cuando agregas una clave SSH al ssh-agent. Si eliges no agregar una frase de contraseña a tu clave, ejecuta el comando sin la opción `--apple-use-keychain`.
   >
   > La opción `--apple-use-keychain` está en la versión estándar de Apple de `ssh-add`. En las versiones de macOS anteriores a Monterey (12.0), las marcas `--apple-use-keychain` y `--apple-load-keychain` usaban la sintaxis `-K` y `-A`, respectivamente.
   >
   > Si no tienes instalada la versión estándar de Apple de `ssh-add`, recibirás un mensaje de error. Para más información, consulta [Error: ssh-add: opción ilegal -- apple-use-keychain](/es/authentication/troubleshooting-ssh/error-ssh-add-illegal-option----apple-use-keychain).
   >
   > Si te sigue pidiendo la frase de contraseña, es posible que tengas que agregar el comando al archivo `~/.zshrc` (o el archivo `~/.bashrc` de Bash).

4. Agrega la clave pública de SSH a tu cuenta en GitHub. Para más información, consulta [Agregar una clave SSH nueva a tu cuenta de GitHub](/es/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

</div>

<div class="ghd-tool windows">

Si ha instalado [GitHub Desktop](https://desktop.github.com/), puede usarlo para clonar repositorios y no tener que utilizar claves SSH.

1. En una nueva ventana de PowerShell con *privilegios elevados de administrador*, asegúrate de que el agente ssh se esté ejecutando. Puedes usar las instrucciones de "Auto-lanzamiento ssh-agent" en [Trabajar con contraseñas de clave SSH](/es/articles/working-with-ssh-key-passphrases) o iniciarla manualmente:

   ```powershell
   # start the ssh-agent in the background
   Get-Service -Name ssh-agent | Set-Service -StartupType Manual
   Start-Service ssh-agent
   ```

2. En una ventana de terminal sin permisos elevados, agregue la clave privada SSH al agente ssh.
   Si has creado tu clave con otro nombre o si vas a agregar una clave existente que tiene otro nombre, reemplaza *id\_ed25519* en el comando por el nombre de tu archivo de clave privada.

   ```powershell
   ssh-add c:/Users/YOU/.ssh/id_ed25519
   ```

3. Agrega la clave pública de SSH a tu cuenta en GitHub. Para más información, consulta [Agregar una clave SSH nueva a tu cuenta de GitHub](/es/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

</div>

<div class="ghd-tool linux">

1. Inicia el agente SSH en segundo plano.

   ```shell
   $ eval "$(ssh-agent -s)"
   > Agent pid 59566
   ```

   Dependiendo de tu ambiente, puede que necesites utilizar un comando diferente. Por ejemplo, es posible que tenga que usar el acceso raíz mediante la ejecución de `sudo -s -H` antes de iniciar ssh-agent, o bien que tenga que usar `exec ssh-agent bash` o `exec ssh-agent zsh` ejecutar ssh-agent.

2. Agrega tu llave privada SSH al ssh-agent.

   Si has creado tu clave con otro nombre o si vas a agregar una clave existente que tiene otro nombre, reemplaza *id\_ed25519* en el comando por el nombre de tu archivo de clave privada.

   ```shell
   ssh-add ~/.ssh/id_ed25519
   ```

3. Agrega la clave pública de SSH a tu cuenta en GitHub. Para más información, consulta [Agregar una clave SSH nueva a tu cuenta de GitHub](/es/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

</div>

## Generar una llave SSH nueva para una llave de seguridad de hardware

Si estás utilizando macOS o Linux, puede que necesites actualizar tu cliente SSH o instalar un cliente SSH nuevo antes de generar una llave SSH nueva. Para más información, consulta [Error: Unknown key type](/es/authentication/troubleshooting-ssh/error-unknown-key-type).

1. Insterta tu clave de seguridad de hardware en tu computadora.

2. Abra <span class="platform-mac">Terminal</span><span class="platform-linux">Terminal</span><span class="platform-windows">Git Bash</span>.

3. Pega el texto siguiente, reemplazando la dirección de correo electrónico usada en el ejemplo por la dirección de correo electrónico asociada a tu cuenta en GitHub.

   <div class="ghd-tool mac">

   ```shell
   ssh-keygen -t ed25519-sk -C "your_email@example.com"
   ```

   </div>

   <div class="ghd-tool windows">

   ```powershell
   ssh-keygen -t ed25519-sk -C "your_email@example.com"
   ```

   </div>

   <div class="ghd-tool linux">

   ```shell
   ssh-keygen -t ed25519-sk -C "your_email@example.com"
   ```

   </div>

   > \[!NOTE]
   > Si se produce un error en el comando y recibes los errores `invalid format` o `feature not supported,` puede que estés usando una clave de seguridad de hardware que no admite el algoritmo Ed25519. En vez de esto, ingresa el siguiente comando.
   >
   > ```shell
   > ssh-keygen -t ecdsa-sk -C "your_email@example.com"
   > ```

4. Cuando se te solicite, pulsa el botón en tu llave de seguridad de hardware.

5. Cuando se te pida "Ingresar un archivo en donde se pueda guardar la llave", teclea Enter para aceptar la ubicación predeterminada.

   <div class="ghd-tool mac">

   ```shell
   > Enter a file in which to save the key (/Users/YOU/.ssh/id_ed25519_sk): [Press enter]
   ```

   </div>

   <div class="ghd-tool windows">

   ```shell
   > Enter a file in which to save the key (c:\Users\YOU\.ssh\id_ed25519_sk):[Press enter]
   ```

   </div>

   <div class="ghd-tool linux">

   ```shell
   > Enter a file in which to save the key (/home/YOU/.ssh/id_ed25519_sk):[Press enter]
   ```

   </div>

6. Cuando se te pida que escribas una frase de contraseña, presiona **Entrar**.

   ```shell
   > Enter passphrase (empty for no passphrase): [Type a passphrase]
   > Enter same passphrase again: [Type passphrase again]
   ```

7. Agrega la clave pública de SSH a tu cuenta en GitHub. Para más información, consulta [Agregar una clave SSH nueva a tu cuenta de GitHub](/es/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).