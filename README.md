# Remote Commander R26 | Descargas para Windows

**[Descargar RemoteCommander-R26-Windows-x64.zip](https://github.com/b414m/remote-commander-downloads/releases/latest)**

Distribución **binaria** de Remote Commander b414m con Desktop Commander MIT 0.3.1 integrado. La instalación es local, no requiere acceso al repositorio privado de desarrollo, GitHub CLI, cuentas de otros usuarios, dominio propio, Tailscale ni servicios Windows.

## Requisitos previos

- Windows 10/11 **x64**, PowerShell 5.1+ y [Node.js 22.12+](https://nodejs.org/).
- **ChatGPT con compatibilidad real** para crear un servidor MCP personalizado mediante **Plugins → + → Add custom MCP server → Connection: Tunnel**. **Plus por sí solo no garantiza esta característica**: funcionó en una cuenta Plus particular, pero el soporte MCP completo documentado oficialmente no está disponible de forma general para Plus. [Documentación de OpenAI](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt).
- Una **organización propia de OpenAI Platform** con permisos para crear un túnel (**Tunnels Read + Manage**) y una clave API **Restricted** para ejecutarlo (**Tunnels Read + Use**). ChatGPT Plus y OpenAI Platform son accesos distintos. [OpenAI Platform Tunnels](https://platform.openai.com/settings/organization/tunnels).

**Cada usuario y cada computadora crean sus propios túneles y claves.** No introduzcas claves de OpenAI en ChatGPT, GitHub ni archivos compartidos.

## Cinco pasos

1. Descarga el ZIP de [Releases](https://github.com/b414m/remote-commander-downloads/releases/latest) y verifica su SHA-256 con el archivo `RemoteCommander-R26-Windows-x64.sha256`. Descomprímelo y abre PowerShell en esa carpeta.

2. Instala y confirma el emparejamiento local una vez:

   ~~~powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Install-B414m.ps1
   ~~~

   Opcionalmente, añade `-AllowTerminal` en **la instalación inicial** si autorizas a ChatGPT a ejecutar comandos con todos los privilegios de tu usuario Windows. **No es un sandbox**. La carpeta predeterminada es `%USERPROFILE%\Projects`; puedes usar `-ProjectsRoot 'D:\Proyectos'`.

3. **Personalmente**, crea un túnel en OpenAI Platform y genera la clave Restricted con **Tunnels Read + Use**. Conserva su `tunnel_id`. El instalador **no genera** túneles, llaves ni permisos en OpenAI.

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

## Integridad y seguridad

El ZIP incluye licencia MIT, avisos de terceros y un manifiesto de inventario; cada release adjunta el hash SHA-256 para comparar antes de ejecutar. El motor actúa con los privilegios de Windows del equipo emparejado. Las restricciones de directorios son controles de acceso de la aplicación, **no** aislamiento del sistema operativo; una terminal arbitraria puede acceder a más rutas.

Este repositorio solo publica **instrucciones y archivos de distribución**. El repositorio de desarrollo `b414m/remote-commander` permanece privado. No se incluyen contraseñas, tokens, emparejamientos, claves de OpenAI Platform ni credenciales DPAPI en el ZIP.
