# Remote Commander R26 v0.11.1 | Descargas para Windows

**[Descargar RemoteCommander-R26-Windows-x64.zip](https://github.com/b414m/remote-commander-downloads/releases/latest)**

Distribución **binaria** de Remote Commander b414m con Desktop Commander MIT 0.3.1 integrado. La instalación es local, no requiere acceso al repositorio privado de desarrollo, GitHub CLI, cuentas de otros usuarios, dominio propio, Tailscale ni servicios Windows.

## ¿Instalar o actualizar?

| Situación | Sigue esta guía | Túnel y clave |
| --- | --- | --- |
| **Primera vez** en esta computadora y usuario Windows | **[Instalar](#instalar-en-una-pc-nueva)** | Crea un túnel y una clave propios. |
| **Ya tienes Remote Commander funcionando** en esta misma computadora y usuario Windows | **[Actualizar](#actualizar-una-instalacion-existente)** | Conserva el túnel, la URL HTTPS, la llave DPAPI y las autorizaciones. |

**Importante:** no uses los pasos de «Instalar» para reemplazar una instalación existente. En «Actualizar» se prepara la nueva versión **antes de detener la anterior**, se realiza el cambio con Ctrl+C y se mantiene disponible la reversión.

## Requisitos comunes y de instalación nueva

- Windows 10/11 **x64**, PowerShell 5.1+ y [Node.js 22.12+](https://nodejs.org/).
- **ChatGPT con compatibilidad real** para crear un servidor MCP personalizado mediante **Plugins → + → Add custom MCP server → Connection: Tunnel**. **Plus por sí solo no garantiza esta característica**: funcionó en una cuenta Plus particular, pero el soporte MCP completo documentado oficialmente no está disponible de forma general para Plus. [Documentación de OpenAI](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt).
- Una **organización propia de OpenAI Platform** con permisos para crear un túnel (**Tunnels Read + Manage**) y una clave API **Restricted** para ejecutarlo (**Tunnels Read + Use**). ChatGPT Plus y OpenAI Platform son accesos distintos. [OpenAI Platform Tunnels](https://platform.openai.com/settings/organization/tunnels).

**Al instalar en una computadora nueva**, cada usuario y cada equipo deben crear su propio túnel y clave. **Al actualizar en el mismo equipo y cuenta Windows se reutilizan los ya configurados.** No introduzcas claves de OpenAI en ChatGPT, GitHub ni archivos compartidos.

## Instalar en una PC nueva

**Solo si es una instalación nueva.** Si ya tienes el MCP configurado, ve directamente a [Actualizar](#actualizar-una-instalacion-existente).

1. Descarga el ZIP de [Releases](https://github.com/b414m/remote-commander-downloads/releases/latest) y verifica su SHA-256 con el archivo `RemoteCommander-R26-Windows-x64.sha256`. Descomprímelo y abre PowerShell en esa carpeta.

**Verificación rápida del ZIP descargado:** desde PowerShell en la carpeta de descargas, ejecuta `Get-FileHash .\RemoteCommander-R26-Windows-x64.zip -Algorithm SHA256`. Su valor debe ser `01b13b284a934e9acb3e2bc4e1da608acaeed2ba2933abb178c23bb478cf7213`; si no coincide, **no ejecutes** el instalador.

2. Instala y confirma el emparejamiento local una vez:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Install-B414m.ps1
   ~~~

   Opcionalmente, añade `-AllowTerminal` en **la instalación inicial** si autorizas a ChatGPT a ejecutar comandos con todos los privilegios de tu usuario Windows. **No es un sandbox**. La carpeta predeterminada es `%USERPROFILE%\Projects`; puedes usar `-ProjectsRoot 'D:\Proyectos'`.

3. **Personalmente**, en [OpenAI Platform → Tunnels](https://platform.openai.com/settings/organization/tunnels) selecciona tu organización y crea un túnel nuevo para esa PC. Copia su `tunnel_id`. Después entra en **Configuración de organización → Proyecto → Claves de API → Crear nueva clave secreta**, elige **Restricted** (no permisos completos) y concede **Tunnels Read + Use** a esa clave. Tu usuario/organización también debe tener esos permisos sobre el túnel. Si no ves la opción Tunnels o esos permisos, tu acceso de Platform no está habilitado y ChatGPT Plus no lo suple. El instalador **no genera** túneles, llaves ni permisos en OpenAI. [Cómo crear claves restringidas](https://help.openai.com/es-419/articles/9186755-managing-projects-in-the-api-platform).

4. Abre dos terminales en la carpeta extraída. Mantén ambas abiertas:

   **Primera terminal:**
   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Start-B414m.ps1
   ~~~

   **Segunda terminal:**
   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Connect-B414mOpenAITunnel.ps1
   ~~~

   En la segunda, introduce el `tunnel_id` y la clave Restricted cuando se soliciten **localmente**. La clave se protege mediante Windows DPAPI. Para sesiones posteriores los asistentes reutilizan la configuración del mismo equipo.

5. Desde **tu propia cuenta de ChatGPT** en la web, crea el complemento MCP personalizado con conexión **Tunnel**, introduce el `tunnel_id` de **tu propia computadora**, acepta el consentimiento y comprueba:

   > Ejecuta remote_hostname y workspace_roots usando mi Remote Commander.

La instalación no modifica otra PC ni instala servicios o tareas programadas. Si la interfaz de ChatGPT no muestra **Connection: Tunnel**, verifica la elegibilidad de tu cuenta; no debes interpretar una suscripción Plus como garantía.

## Actualizar una instalación existente

**No crees otro túnel, URL ni llave:** esta ruta es para el mismo equipo y usuario Windows donde Remote Commander ya está configurado. La versión v0.11.1 reutiliza el túnel de OpenAI, el secreto DPAPI, el emparejamiento, las rutas de proyectos y las autorizaciones. **Descarga el ZIP nuevo y extráelo en una carpeta nueva**, sin borrar la carpeta de la versión anterior.

1. **Prepara R26 con el MCP anterior todavía funcionando.** En PowerShell abierto dentro de la carpeta del ZIP **nuevo** ejecuta:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Install-B414m.ps1 -Upgrade
   ~~~

   Se verifica el paquete y se crea una configuración alternativa y copia privada de estado. **No detiene el MCP actual ni altera su configuración original.** Ejecutar otra vez `-Upgrade` con el plan ya preparado es seguro.

2. **Pulsa Ctrl+C en la ventana de PowerShell que mantiene la versión anterior.** Si el servidor y `tunnel-client` están en **dos ventanas**, detén **ambos** con Ctrl+C. Si ambos se lanzaron juntos mediante `local-stack run --with-tunnel`, basta con detener esa ventana. ChatGPT perderá temporalmente el enlace: **el cambio se realiza en Windows, no desde el propio MCP**.

3. **Activa R26** desde la carpeta del ZIP **nuevo**, dejando abierta la terminal:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Upgrade-B414m.ps1 -Action Activate
   ~~~

   R26 usa el mismo puerto local 8787, túnel, dirección HTTPS y llave existente. **No cambies el complemento ni el túnel en ChatGPT.** Si el puerto o el túnel todavía están ocupados, el instalador bloquea el arranque en lugar de duplicarlos.

4. **Verifica la conexión** desde ChatGPT con `remote_hostname` y `workspace_roots`, y revisa tus capacidades y autorizaciones antes de ejecutar modificaciones.

**Si falla y necesitas revertir:** detén R26 con **Ctrl+C** y ejecuta desde la misma carpeta del ZIP nuevo:

~~~powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Upgrade-B414m.ps1 -Action Rollback
~~~

Se inicia el ejecutable anterior con su configuración original, sin regenerar ni copiar llaves. Para comprobar el plan sin activar el MCP ejecuta `powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Upgrade-B414m.ps1 -Action Status`. Si R26 ya modificó datos persistentes, evalúa su compatibilidad al regresar a versiones antiguas.

**Otro equipo o cuenta Windows no es «Actualizar».** Sigue «Instalar», porque DPAPI no permite trasladar credenciales protegidas entre perfiles o equipos y nunca deben reutilizarse las claves de otra persona. No se conceden permisos nuevos automáticamente.

## Integridad y seguridad

El ZIP incluye licencia MIT, avisos de terceros y un manifiesto de inventario; cada release adjunta el hash SHA-256 para comparar antes de ejecutar. El motor actúa con los privilegios de Windows del equipo emparejado. Las restricciones de directorios son controles de acceso de la aplicación, **no** aislamiento del sistema operativo; una terminal arbitraria puede acceder a más rutas.

Este repositorio solo publica **instrucciones y archivos de distribución**. El repositorio de desarrollo `b414m/remote-commander` permanece privado. No se incluyen contraseñas, tokens, emparejamientos, claves de OpenAI Platform ni credenciales DPAPI en el ZIP.
