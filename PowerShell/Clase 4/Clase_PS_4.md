## EJERCICIOS
**1. Mostrar una tabla de procesos que incluya únicamente los nombres de los
   procesos, sus IDs, y si están respondiendo a Windows (la propiedad
   ``Responding`` muestra eso). Haga que la tabla tome el mínimo de espacio
   horizontal, pero no permita que la información se trunque.**

	**R/** Se utilizó el comando para formateo en forma de tabla con parámetros 
	correspondientes a los atributos especificados, se incorpora el
	parámetro -Wrap para que no se trunque la información y el parámetro
	-AutoSize para que las columnas se ajusten a su contenido.

	``Get-Process | ft Name,ID,Responding -Wrap -AutoSize``

**2. Muestre una tabla de procesos que incluya los nombres de los procesos y sus
   IDs. También incluya columnas para uso de memoria virtual y física;
   exprese dichos valores en megabytes (MB).**

   **R/** Se utilizó el comando para formateo en forma de tabla con parámetros 
	correspondientes a los atributos especificados, se especifican personalizadamente las columnas Vm y Pm con la notación @{n;e} y se dividen sus valores entre 1MB y se castean a int.

   ``Get-Process | ft ID, Name ,@{n='Vm (MB)';e={$_.Vm / 1MB -as [int]}}, @{n='Pm (MB)';e={$_.Pm / 1MB -as [int]}}``

**3. Emplee ``Get-EventLog`` para mostrar una lista de los logs de eventos
   disponibles (revise la ayuda para encontrar el parámetro que le permitirá
   obtener dicha información). Formatee la salida como una tabla que incluya
   el nombre de despliegue del log y el período de retención. Los encabezados
   de columna deben ser NombreLog y Per-Retencion.**

   **R/**  Primero se usó el parámetro -list para listar los eventlogs, el resultado se le pasó al comando gm para encontrar el nombre verdadero de los atributos pedidos. Se utilizó el comando para formateo en forma de tabla con parámetros 
	correspondientes a los atributos especificados, se personalizaron las columnas para darles el nombre que se pedía mediante la notación @{n;e}.

   ``Get-EventLog -List | ft @{n='NombreLog';e={$_.LogDisplayName}}, @{n='Per-Retencion';e={$_.MinimumRetentionDays}}``

**4. Muestre una lista de servicios, de tal manera que aparezcan agrupados los
   servicios que están iniciados y los que están detenidos. Los que están
   iniciados deben aparecer primero.**

   **R/** Primero se obtuvieron los servicios, luego se ordenaron descendientemente como se pedia, por último se les dio formato de tabla y se le agrupó por su atributo de estatus

   ``Get-Service | sort Status -Descending | ft -GroupBy Status``

**5. Mostrar una lista a cuatro columnas de todos los directorios que est�n en
   el raíz de la unidad ``C:``**

   **R/** Se usó el comando dir y el parámetro -path seguido de la locación especificada, el resultado se le entrega al comando fw cuyo parámetro -Column fue especificado como 4

   ``dir -Path 'C:' | fw -Column 4``

**6. Cree una lista formateada de todos los archivos ``.exe`` del directorio
   ``C:\Windows``. Debe mostrarse el nombre, la información de versión, y el
   tamaño del archivo. La propiedad de tamaño se llama ``length`` en Powershell,
   pero para mayor claridad, la columna se debe llamar **Tamaño** en su listado.**

   **R/** Se uso el comando dir para encontrar los archivos del directorio especificado, se filtraron por la extensión .exe y se dio el formato de lista especificado

   ``dir -Path C:\Windows | where -filter {$_.Extension -eq '.exe'} | fl Name, @{n='Tamaño';e={$_.Length}}, VersionInfo``

**7. Importe el módulo ``NetAdapter`` (empleando el comando ``Import-Module
   NetAdapter``).
   Empleando el cmdlet ``Get-NetAdapter``, muestre una lista de adaptadores no
   virtuales (adaptadores cuya propiedad Virtual sea falsa. El valor lógico
   falso es representado por Powershell como ``$False``).**

   **R/** Se usó un filtro donde se busca que la propiedad Virtual sea falsa

   ``Get-NetAdapter | where -filter {$_.Virtual -eq $false}``

**8. Importe el módulo ``DnsClient``. Empleando el cmdlet ``Get-DnsClientCache``,
   muestre una lista de los registros ``A`` y ``AAAA`` que están en el caché.
   Sugerencia: Si el caché está vacío, visite algunos sitios web para poblarlo.**

   **R/** Se usó un filtro donde es espeficaba que el atributo type podía ser A o AAAA.

   ``Get-DnsClientCache | where -filter {$_.Type -like 1 -or $_.Type -like 28}``

**9. Genere una lista de todos los archivos ``.exe`` del directorio
   ``C:\Windows\System32`` que tengan más de 5 MB.**

   **R/** Se usó un filtro para encontrar los archivos exe que tuvieran tamaño mayor a 5MB para esto se hizo la conversión de los tamaños a MB diviendo el tamaño por 1MB.

   ``dir -Path C:\Windows\System32 | where -filter {$_.Extension -eq '.exe' -and $_.Length/1MB -gt 5} | ft Name, @{n='Length (MB)';e={$_.Length / 1MB -as [int]}}``

**10. Muestre una lista de parches que sean actualizaciones de seguridad.**
    
   **R/** Se filtraron los hotfixes por medio de la descripción.

   ``Get-HotFix | where -filter {$_.Description -eq 'Security Update'}``


**11. Muestre una lista de parches que hayan sido instalados por el
    usuario ``Administrador``, que sean actualizaciones. Si no tiene ninguno,
    busque parches instalados por el usuario ``System``. Note que algunos parches
    no tienen valor en el campo ``Installed By``.**

    **R/** Se hizo un filtro sobre las propiedades installby y la descripción.

    ``Get-HotFix | where -filter {$_.InstalledBy -eq 'NT AUTHORITY\SYSTEM' -and $_.Description -match 'Update'}``

**12. Genere una lista de todos los procesos que están corriendo con el nombre
    **Conhost** o **Svchost**.**
    
    **R/** Se hizo un filtro sobre la propiedad nombre.

    ``Get-Process | where -filter {$_.Name -eq 'Conhost' -or $_.Name -eq 'SvcHost'}``