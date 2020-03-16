## EJERCICIOS
**1. ¿Cuál clase puede emplearse para consultar la dirección IP de un adaptador
   de red? ¿Posee dicha clase algún método para liberar un préstamo de
   dirección (lease) DHCP?**

   **R** La clase win32_NetworkAdapterConfiguration puede emplearse para consultar la IP de un adaptador mediante la propiedad IPAddres.

   ``Get-CimInstance -ClassName win32_NetworkAdapterConfiguration | ft ServiceName, IPAddress``

    Al parecer no se encuentran métodos para liberar el préstamo de dirección al buscarlo con los comandos:

   ``Get-CimInstance -ClassName CIM_NetworkAdapter | where Name -EQ 'Microsoft Wi-Fi Direct Virtual Adapter' | gm -MemberType Method``

   ``Get-CimInstance -ClassName win32_NetworkAdapterConfiguration | gm -MemberType Method``

   Sin embargo, investigando en la red encontré el método releaseDHCPLease(), el cuál no aparece en la lista de métodos pero en la red se afirma que existe y que se usa exactamente para liberar una IP dada por el servidor DHCP


**2. Despliegue una lista de parches empleando WMI (Microsoft se refiere a los
   parches con el nombre **quick-fix engineering**). Es diferente el listado al
   que produce el cmdlet ``Get-Hotfix``?**

   **R/**Para obtener la lita con los parches con el nombre especificado se utilizó.

   ``Get-WmiObject -List | where name -Like '*fix*' | ft -Property *``

   Se obtuvo un único elemento **Win32_QuickFixEngineering**, el listado es diferente al producido por Get-Hotfix como lo muestra el transcript.

**3. Empleando WMI, muestre una lista de servicios, que incluya su status actual,
   su modalidad de inicio, y las cuentas que emplean para hacer login.**

   **R/** 
 
   ``Get-WmiObject -Class Win32_Service | ft Status, StartMode, Name``



**4. Empleando cmdlets de CIM, liste todas las clases del namespace
   ``SecurityCenter2``, que tengan **product** como parte del nombre.**

   **R/**

   ``Get-CimClass -Namespace root\CIMv2 | where cimclassname -Like '*product*'``

**5. Empleando cmdlets de CIM, y los resultados del ejercicio anterior, muestre
   los nombres de las aplicaciones antispyware instaladas en el sistema.
   También puede consultar si hay productos antivirus instalados en el sistema.**

   **R/** Se trató de encontrar las aplicaciones antispyware instaladas con el comando:

   ``Get-CimClass -Namespace root\CIMv2 | where cimclassname -Like '*AntiSpyware*'``

    Sin embargo no hubo resultado, por lo tanto concluyo que en mi equipo no hay instalados programas antispyware

    Se trato de encontrar igualmente productos antivirus instalados con el comando:

    ``Get-CimClass -Namespace root\CIMv2 | where cimclassname -Like '*antivirus*'``

    Sin embargo no hubo resultado, concluyo igualmente que no hay instalado un antivirus en mi equipo