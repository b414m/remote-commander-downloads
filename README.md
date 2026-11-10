# Remote Commander R26 v0.11.1 | Descargas para Windows

**[Descargar RemoteCommander-R26-Windows-x64.zip](https://github.com/b414m/remote-commander-downloads/releases/latest)**

Distribución **binaria** de Remote Commander b414m con Desktop Commander MIT 0.3.1 integrado. La instalación es local, no requiere acceso al repositorio privado de desarrollo, GitHub CLI, cuentas de otros usuarios, dominio propio, Tailscale ni servicios Windows.

## Requisitos previos

- Windows 10/11 **x64**, PowerShell 5.1+ y [Node.js 22.12+](https://nodejs.org/).
- **ChatGPT con compatibilidad real** para crear un servidor MCP personalizado mediante **Plugins → + → Add custom MCP server → Connection: Tunnel**. **Plus por sí solo no garantiza esta característica**: funcionó en una cuenta Plus particular, pero el soporte MCP completo documentado oficialmente no está disponible de forma general para Plus. [Documentación de OpenAI](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt).
- Una **organización propia de OpenAI Platform** con permisos para crear un túnel (**Tunnels Read + Manage**) y una clave API **Restricted** para ejecutarlo (**Tunnels Read + Use**). ChatGPT Plus y OpenAI Platform son accesos distintos. [OpenAI Platform Tunnels](https://platform.openai.com/settings/organization/tunnels).

**Cada usuario y cada computadora crean sus propios túneles y claves.** No introduzcas claves de OpenAI en ChatGPT, GitHub ni archivos compartidos.

## Cinco pasos

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

## Actualizar Remote Commander en una PC donde ya funciona

**No necesitas crear otro túnel, URL ni clave si actualizas en el mismo usuario de Windows.** La versión v0.11.1 reutiliza el túnel actual, la llave protegida por DPAPI y las autorizaciones existentes, sin sobrescribir la configuración anterior.

1. Descarga el ZIP nuevo, verifica su hash y extráelo en una carpeta nueva. Con la versión anterior todavía funcionando, ejecuta desde esa carpeta:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Install-B414m.ps1 -Upgrade
   ~~~

   La preparación verifica el paquete, guarda una copia privada del estado y configura el nuevo motor. **No detiene el servicio anterior**.

2. En la terminal del Remote Commander antiguo pulsa **Ctrl+C** para detener su proceso y túnel. A continuación, en la carpeta del ZIP nuevo, ejecuta:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Upgrade-B414m.ps1 -Action Activate
   ~~~

3. Comprueba en ChatGPT que el mismo complemento vuelve a responder. Si necesitas revertirlo, pulsa Ctrl+C en la terminal de R26 y ejecuta:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Upgrade-B414m.ps1 -Action Rollback
   ~~~

**La dirección HTTPS y el tunnel_id se mantienen.** No copies ni pegues las claves en ChatGPT. El cambio de procesos siempre se ejecuta desde Windows, pues al detener el túnel ChatGPT perderá temporalmente la conexión. Este procedimiento funciona con instalaciones compatibles en el mismo usuario de Windows; las instalaciones nuevas en otro equipo o perfil requieren un túnel y llave propios. No se automatiza ninguna concesión adicional de permisos.

## Integridad y seguridad

El ZIP incluye licencia MIT, avisos de terceros y un manifiesto de inventario; cada release adjunta el hash SHA-256 para comparar antes de ejecutar. El motor actúa con los privilegios de Windows del equipo emparejado. Las restricciones de directorios son controles de acceso de la aplicación, **no** aislamiento del sistema operativo; una terminal arbitraria puede acceder a más rutas.

Este repositorio solo publica **instrucciones y archivos de distribución**. El repositorio de desarrollo `b414m/remote-commander` permanece privado. No se incluyen contraseñas, tokens, emparejamientos, claves de OpenAI Platform ni credenciales DPAPI en el ZIP.
