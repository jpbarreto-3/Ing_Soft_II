**STRIDE**



Es un modelo de análisis de amenazas desarrollado por Microsoft. Su funcionalidad es, principalmente, identificar y clasificar riesgos de ciberseguridad en sistemas y aplicaciones.



Se compone de seis categorias o "frentes" que se deben tener asegurados por diferentes tipos de ataques de los cuales se puede ser victima. A continuación, se presentan detalladamente sus significados y relevancia además de técnicas de mitigación de riesgo dadas por auditores de ciberseguridad.



**S --> Spoofing**



Su traducción directa del inglés es : "Suplantación  de identidad"



Hace referencia al ataque en el que se busca falsificar datos tales como: Direcciones IP, correos electrónicos o sitios web. Sin embargo, los atacantes procuran que la falsificación luzca muy realista para que las victimas caigan en la trampa.



El objetivo principal es robar datos sensibles como contraseñas y datos personales de los usuarios, además de instalar malware o robar dinero. 



Para mitigar este ataque se deben tener en cuenta varias cosas. Desde la etapa de desarrollo de una aplicación o sitio web los desarrolladores deben incluir protocolos de autenticación de correo electrónico como:



SPF (Sender Policy Framework) : Mecanismo de autenticación que permite a un dominio autorizar qué servidores pueden enviar emails en su nombre. Es decir, bloquea correos ilegítimos y evita que los legítimos no queden como spam (Falso positivo).



DKIM ( DomainKeys Identified Mail) : Mecanismo de autenticación de correo que usa firmas criptográficas para garantizar que un email NO fue modificado y que fue autorizado por el dominio emisor. 



DMARC (Domain-based Message Authentication, Reporting \& Conformance) : Politica de correo que le dice al servidor receptor qué hacer cuando un email falla SPF y/o DKIM. También permite recibir reportes sobre esos fallos.



También se deben usar filtros de seguridad en redes como firewalls o dobles factores de autenticación para mitigar la facilidad de acceso a cuentas cuando la contraseña fue expuesta. 



Para concluir, también es imprescindible la capacitación al personal sobre correos maliciosos y técnicas para identificarlos. Incluyendo el Cero Trust y la revisión de sitios webs seguros que apliquen HTTPS.



**T --> Tampering**



Su traducción directa del inglés es : "Manipulación"



Puede parecer similar a la anterior, spoofing, sin embargo, se diferencia de esta porque en el tampering se busca atacar principalmente la integridad de la información. Un ejemplo de esto, es que se intercepta un email legítimo pero se altera su contenido.



Los atacantes buscan realizar este ataque con el fin de lograr fraudes, redirección maliciosa, escalada de privilegios, sabotaje, etc. No busca alterar la identidad del emisor, por el contrario, se mantiene la identidad original pero se altera lo que este originalmente envió.



Con el fin de mitigar estos ataques se debe implementar:



Firmas digitales como DKIM (Vista anteriormente) y HMAC que utilizan Hash para mayor seguridad



Cifrar los datos en transporte por medio de: TLS, HTTPS, STARTTLS 



Cifrado end-to-end que indica que la información está cifrada entre el emisor y receptor. Para esto se usa PGP, S/MIME



Validación de integridad en la aplicación por medio de checksums, tokens firmados y validación rigurosa.



**R --> Repudiation**



Su traducción directa del inglés es : "Repudio"



Este ataque explota la falta de trazabilidad y el no repudio. Básicamente se busca negar una acción (que sí se hizo) por la falta de capacidad del sistema de probar lo que se hizo, cuándo y cómo.



Los atacantes buscan esto con el fin de negar fraudes, evitar responsabilidad legal, borrar huellas, y en general abusar de sistemas débiles. Como consultores, lo que se debe implementar con el fin de mitigar las posibilidades que esto suceda son las siguientes:



Firmas digitales como se ha mencionado anteriormente usando DKIM y certificados



Logs inmutables y auditables: Se entiende inmutable como aquello que no puede ser cambiado ni eliminado una vez se ha escrito. Una de las estrategias es usar "Append-Only" lo que permite que el sistema sólo tenga la posibilidad de añadir nuevos registros a una base de datos o archivo, pero que estén restringidas las funciones de actualización o eliminación de algún dato. Un log se entiende como una "bitácora" donde se registra cronológicamente todas las actividades, errores o transacciones que ocurren en un software, servidor o red. Para evitar posibles alteraciones sobre los logs se usan las firmas digitales que permite cifrar y por medio de llaves revisar la procedencia del log. Muy importante es también el "cuándo", por eso se usa un Time-Stamping confiable tales como TSA o el RFC 3161 que permite garantizar que el tiempo registrado en el log es auténtico.



También es importante el monitoreo y auditoría de los logs con el fin de revisar algún log eliminado o alterado.



En general, el sistema debería siempre tener una trazabilidad que permita responder a las dudas de qué, cómo, cuándo de manera confiable.



**I --> Information Disclosure**



Su traducción directa del inglés es : "Divulgación de información"



En términos simples es cuando una información sensible o confidencial se expone a personas no autorizadas. Los criminales al tener acceso a información de este tipo buscan robar datos sensibles, multiplicar el ataque aumentando sus privilegios dentro de un sistema, realizar espionaje y en general explotar esto con el fin de lograr la extorsión y el fraude.



Como auditores, se implementan las siguientes medidas para evitar este acceso no-autorizado.



Inicialmente diferenciar de los datos en tránsito y en reposo. Cuando están en reposo, es decir, almacenados en discos, bases de datos o backups se debe tener un cifrado adecuado y, evidentemente, buena gestión de claves y controles de acceso. Cuando la información se encuentra en tránsito entre receptor y remitente también debe ir cifrada por medio de TLS y HTTPS.



Para la gestión del control de acceso de implementa:



RBAC (Role-Based Access Control) : Restringe el acceso a los datos de acuerdo a los   roles definidos de cada usuario dentro de una organización.



ABAC (Attribute-Based Access Control) : Restringe el acceso a los datos de acuerdo no solo a los roles sino de manera más dinámica y flexible. Tiene en cuenta factores como tipo de usuario, hora, contexto de la petición, dirección IP, etc.



Principio de mínimo privilegio : Limita el acceso a usuarios a sistemas o procesos a un nivel estrictamente necesario para realizar sus funciones. Reduce la superficie de ataque y minimiza posibles riesgos humanos



Por otro lado, también se debe evitar hardcodear claves (Poner credenciales directamente en el código), usar vaults de almacenamiento de credenciales y rotarlos.



También, los hackers buscarán la mayor información sobre el sistema previo a su ataque, por ende, se deben sanitizar salidas no exponiendo errores, stack traces o logs sensibles. Finalmente, se debe evitar enviar datos sensibles en texto plano o evitar conexiones en lugares donde la red sea insegura y usar protocolos de cifrado de información.



**D --> Denial of Service (DoS)**



Su traducción directa del inglés es : "Denegación de servicio"



Es un ataque muy común que busca dejar a un sistema indisponible, degradarlo o finalmente que no sea utilizable para usuario legítimos.



Un criminal lo puede aplicar para interrumpir servicios críticos de un sistema, extorsionar, encubrir otros ataques, daño reputacional o competencia desleal.



Su estrategia es simple: Consumir recursos de manera desenfrenada hasta que el sistema no pueda procesarlos y finalmente no responda.



En esta categoria, existen dos modalidades DoS y DDoS. La "D" extra es de "distributed". Indica que el ataque es mucho más agresivo y emplea múltiples sistemas infectados (botnets). 



Para mitigar este ataque se emplea lo siguiente:



Limitación de tasa (Rate Timing): Evita abuso de recursos y requests por IP/Usuario



Balanceo y escalabilidad: Permite distribuir eficientemente el tráfico de la red y cargas de trabajo entre varios servidores. Esto evita sobrecargas y mejora la disponibilidad y eficiencia. También se sugiere el uso de auto-scaling que permite el ajuste eficiente de recursos de manera automática de acuerdo a la demanda en tiempo real.



Implementar protección DDoS dedicada: CDNs son una excelente estrategia ya que distribuye el tráfico entre diferentes nodos cuando se detecta una carga excesiva en un solo puntos. También las WAF que permite filtrar ataques que pueden hacerse pasar por legítimos y bloquear IPs sospechosas. También implementa desafíos CAPTCHA y retos JavaScript con el fin de evitar bots. Una muy eficiente son los scrubbing centers que permiten recibir el ataque y filtrarlo para después devolver lo que es legítimo al sistema.



Aplicar Timeouts y límites: Implementar límite de conexiones que permitan evitar agotamiento de memoria/CPU



Detección por medio de monitoreos y alertas: Es importante que se tenga la capacidad de recibir alertas y un monitoreo con el fin de detectar picos anormales que permitan tomar medidas de mitigación.



**E --> Elevation of Privilege**



Su traducción directa del inglés es : "Escalamiento de privilegios"



Es cuando un atacante obtiene permisos superiores a los que debería tener. Esto sucede, principalmente, por fallos de diseño, configuración o validación. Un ejemplo es un usuario que logre tener permisos de un rol con una jerarquía mayor a la que él tiene. 



Su principal amenaza es que criminales logran tomar control del sistema; acceder a información sensible; realizar modificaciones en configuraciones críticas; lograr añadir cosas para afectar aún más el sistema como la creación de usuarios, backdoors, reglas ocultas, entre otras; Encadenar ataques.



La forma de mitigar esto es aplicando lo siguiente:



Principio del mínimo privilegio: Como se mencionó anteriormente se debe garantizar que un usuario no tenga acceso a aquello que no es lo mínimo requerido para sus funciones



Separación de roles: Dentro de un sistema no se pueden categorizar a todos los usuarios con un mismo rol. Es necesario tenerlos claramente definidos, así como a las cosas a las que tiene acceso cada uno.



Validación de autorización en servidor: Se debe aplicar el Cero Trust, es decir un usuario no puede tomar una decisión crítica sin autorización o validación de otros usuarios. Esto incluye la revisión de permisos en cada acción del sistema.



Hardening del sistema: Permite mitigar el impacto de un ataque por medio de la eliminación de configuraciones inseguras, cierre de puertos e inhabilitación de servicios innecesarios. También implica aplicar parches de seguridad.



Autenticación adecuada: Esto implica autenticación multifactor (MFA), implica que se debe probar la identidad en más de una forma. Se usa códigos OTP, biometría, entre otros. 



Finalmente, se debe tener posibilidad de monitorear y auditar logs de acciones administrativas y revisar posibles privilegios elevados.



**En conclusión**, STRIDE permite evitar ataques de múltiples tipos asegurando la tríada de la seguridad de la información CIA (Confidencialidad, Integridad y Disponibilidad). Además de que permite tomar medidas en cada una de las posibles debilidades de un sistema incluso desde la fase de su diseño y desarrollo.











