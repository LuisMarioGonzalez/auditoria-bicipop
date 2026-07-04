# Guion de Defensa Oral — Auditoría de Seguridad Web BiciPop

### KeepCoding XIX Bootcamp · Versión actualizada para la presentación de 20 diapositivas

### Duración objetivo: 15–17 minutos

> \*\*Cómo utilizarlo:\*\* cada bloque corresponde exactamente a una diapositiva del PowerPoint. El texto está redactado para poder decirlo casi literalmente, pero conviene usarlo como guía y no leerlo. Las indicaciones entre paréntesis son notas de puesta en escena.

> \*\*Nota de estilo:\*\* al tratarse de un trabajo en equipo, el guion utiliza principalmente “nosotros”. Cambiadlo a singular en las partes que exponga una sola persona.

\---

## Slide 1 — Portada  ⏱️ 0:00–0:20

> “Buenos días. Somos Luis Mario y Gustavo, y vamos a presentar nuestro proyecto final del Bootcamp de Ciberseguridad de KeepCoding: una \*\*auditoría de seguridad web sobre BiciPop\*\*, realizada mediante un pentest asistido por inteligencia artificial con \*\*CAI, Cybersecurity AI\*\*.”

\---

## Slide 2 — Presentación del equipo  ⏱️ 0:20–0:55

> “Antes de entrar en la parte técnica, presentamos brevemente el equipo y cómo organizamos el trabajo.”
>
> “Luis Mario se centró principalmente en el reconocimiento, la enumeración y las pruebas activas. \*\*\[Nombre]\*\* trabajó en la verificación de hallazgos y el análisis DNS y de correo. La elaboración del informe, las mitigaciones y la presentación se realizaron conjuntamente.”
>
> “Aunque repartimos las tareas, revisamos en equipo los resultados más importantes, especialmente aquellos generados inicialmente por los agentes de inteligencia artificial.”

\---

## Slide 3 — Pitch inicial: problema y propuesta  ⏱️ 0:55–1:45

> “BiciPop es un marketplace de bicicletas de segunda mano desarrollado dentro del propio bootcamp. La aplicación permite publicar productos, consultar bicicletas y contactar con sus vendedores.”
>
> “Nuestra propuesta fue analizar qué ocurriría si esta aplicación fuese a desplegarse en un entorno real de producción. Para ello planteamos una auditoría con tres objetivos: \*\*identificar vulnerabilidades reales\*\*, clasificarlas por severidad utilizando referencias como CWE y CVSS, y proponer medidas de remediación concretas.”
>
> “Es importante aclarar que se trató de una auditoría \*\*autorizada\*\*, limitada al objetivo designado por el bootcamp y ejecutada mediante pruebas no destructivas.”

\---

## Slide 4 — El proyecto y demostración funcional  ⏱️ 1:45–2:45

> “Antes de hablar de vulnerabilidades, vamos a enseñar brevemente la aplicación para entender su contexto.”
>
> “BiciPop permite navegar por un catálogo de bicicletas, consultar fichas de producto y perfiles de vendedor, publicar nuevos anuncios y utilizar funciones de registro, inicio de sesión y mensajería.”
>
> “En esta demostración haremos un recorrido rápido por el catálogo, una ficha de producto y el flujo de autenticación. No buscamos explicar toda la aplicación, sino mostrar qué información ve un usuario normal y cuál es la superficie funcional que posteriormente auditamos.”

**Durante la demo:**

1. Mostrar la página principal y el catálogo.
2. Abrir una ficha de producto y señalar vendedor, ubicación y precio.
3. Enseñar brevemente registro/login o la pantalla de alta de producto.
4. No dedicar más de un minuto; la demostración de seguridad llegará en la slide 16.

\---

## Slide 5 — Reconocimiento de la infraestructura  ⏱️ 2:45–3:30

> “El reconocimiento permitió identificar la arquitectura principal. El frontend está desarrollado con \*\*Next.js 14\*\*, usando App Router y Turbopack. El backend utiliza \*\*Supabase\*\*, con PostgreSQL y almacenamiento de objetos, y \*\*Prisma\*\* como ORM, empleando identificadores CUID2.”
>
> “La aplicación se encuentra detrás de \*\*nginx\*\*, desplegada en una instancia de \*\*AWS EC2\*\*, y utiliza un certificado TLS válido de Let’s Encrypt.”
>
> “A nivel de red, la superficie expuesta era reducida: ICMP estaba bloqueado y los puertos aparecían filtrados, de modo que el trabajo se concentró principalmente en HTTP y HTTPS, es decir, en la capa de aplicación.”

\---

## Slide 6 — Metodología de pentest  ⏱️ 3:30–4:25

> “Organizamos la auditoría en seis fases.”
>
> “Primero realizamos el \*\*reconocimiento\*\*, identificando tecnologías y comportamiento del servidor. Después analizamos las \*\*cabeceras de seguridad\*\*. En la tercera fase enumeramos rutas, APIs, directorios y archivos potencialmente sensibles.”
>
> “La cuarta fase incluyó pruebas activas de XSS, SQL Injection, IDOR, SSTI y autenticación. La quinta se centró en DNS y seguridad de correo, revisando SPF, DMARC y MX. Finalmente, en la sexta fase, verificamos manualmente cada resultado para confirmar los hallazgos y eliminar falsos positivos.”
>
> “La metodología fue tradicional; la particularidad fue utilizar CAI como orquestador de parte del proceso.”

\---

## Slide 7 — Inteligencia artificial ofensiva: CAI y agentes  ⏱️ 4:25–5:20

> “CAI es un framework que permite trabajar con agentes de inteligencia artificial especializados en tareas ofensivas. En total utilizamos seis agentes, aunque cuatro tuvieron un papel principal.”
>
> “El \*\*agente 20\*\*, Web App Pentester, realizó reconocimiento, enumeración y pruebas iniciales. El \*\*agente 9\*\* se especializó en DNS y SMTP. El \*\*agente 4\*\*, Bug Bounter, ejecutó pruebas dirigidas sobre XSS, login e IDOR. Y el \*\*agente 12\*\*, Red Team, fue especialmente útil para detectar la exposición de datos de usuario.”
>
> “Además, utilizamos el agente 15 para reverificar resultados y el 14 como apoyo en la generación del informe. Los modelos empleados fueron Claude Haiku 4.5 y Claude Sonnet 4.6, ejecutados mediante API sobre Kali Linux.”

\---

## Slide 8 — El proceso paso a paso  ⏱️ 5:20–6:10

> “El proceso comenzó configurando CAI, la clave de API y los modelos de Claude. Después lanzamos el agente de pentest web para obtener una primera visión de tecnologías, cabeceras y rutas.”
>
> “Con esa información pasamos a pruebas más dirigidas: XSS, autenticación e IDOR. Paralelamente, el agente DNS analizó los registros de correo. Posteriormente utilizamos el agente Red Team para buscar combinaciones de hallazgos y exposición de información.”
>
> “La última fase fue la más importante: \*\*repetimos manualmente los comandos y revisamos las respuestas reales del servidor\*\*. CAI se utilizó para generar hipótesis y acelerar el análisis, pero ninguna conclusión se incorporó al informe sin validación independiente.”

\---

## Slide 9 — Problemas y limitaciones de CAI  ⏱️ 6:10–7:10

> “El uso de CAI también presentó problemas importantes. Con los modelos gratuitos, el comportamiento fue poco fiable: repetían pruebas, inventaban identificadores o rutas y, en algunos casos, describían resultados que realmente no se habían producido.”
>
> “También encontramos falsos positivos relacionados con el guardrail de Unicode y bloqueos cuando los agentes intentaban ejecutar comandos largos.”
>
> “Para solucionarlo, utilizamos modelos de pago con mejor capacidad de razonamiento, ajustamos la configuración del archivo `.env` y ejecutamos manualmente algunos comandos desde la terminal cuando CAI se bloqueaba.”
>
> “La principal lección fue clara: una IA ofensiva puede acelerar el trabajo, pero \*\*no sustituye la comprobación técnica del auditor\*\*. La salida del agente es una hipótesis, no una evidencia.”

\---

## Slide 10 — Visión global de resultados  ⏱️ 7:10–7:35

> “El resultado final fue de \*\*siete hallazgos de seguridad\*\*: dos críticos, dos altos, dos medios y uno informativo. Además, verificamos dos controles que funcionaban correctamente, por lo que documentamos nueve comprobaciones en total.”
>
> “A continuación vamos a explicar los cuatro hallazgos más relevantes.”

\---

## Slide 11 — H-01: ausencia de cabeceras de seguridad  ⏱️ 7:35–8:30

> “El primer hallazgo fue la ausencia total de cabeceras de seguridad HTTP recomendadas para endurecer el navegador.”
>
> “Mediante `curl` comprobamos que la aplicación no devolvía cabeceras como \*\*Content-Security-Policy\*\*, \*\*X-Frame-Options\*\*, \*\*Strict-Transport-Security\*\* o \*\*X-Content-Type-Options\*\*.”
>
> “Esta ausencia no crea por sí sola un XSS, pero elimina varias capas de defensa. Por ejemplo, permite que la página pueda ser cargada dentro de un `iframe`, aumentando el riesgo de clickjacking; y, si apareciese una vulnerabilidad XSS, la falta de CSP ampliaría su impacto. La ausencia de HSTS también elimina la protección del navegador frente a determinados intentos de degradación de HTTPS.”
>
> “La corrección es relativamente sencilla: configurar estas cabeceras en nginx y probar una política CSP adaptada a los recursos reales de la aplicación.”

\---

## Slide 12 — H-02: dominio vulnerable a email spoofing  ⏱️ 8:30–9:25

> “El segundo hallazgo crítico se encuentra en la configuración DNS del correo.”
>
> “El dominio no presentaba un registro SPF válido y el registro DMARC existía, pero estaba vacío. Por tanto, los servidores receptores no disponían de una política clara para identificar qué sistemas podían enviar correo en nombre de BiciPop ni qué hacer cuando una validación fallase.”
>
> “Esto aumenta significativamente el riesgo de \*\*suplantación del remitente\*\*, phishing e ingeniería social contra usuarios del marketplace.”
>
> “La recomendación es definir los servidores autorizados en SPF y terminar la política con un mecanismo restrictivo. Si el dominio no debe enviar correo, puede utilizarse `-all`. DMARC debería configurarse primero con monitorización y evolucionar después hacia `quarantine` o `reject`.”

\---

## Slide 13 — H-03: exposición de datos de usuario e IDs internos  ⏱️ 9:25–10:35

> “Este fue el hallazgo más interesante de la auditoría y fue detectado durante las pruebas del agente Red Team.”
>
> “El endpoint público `/search` devolvía, dentro del payload de \*\*React Server Components\*\*, la estructura de cada producto. Entre los campos enviados al navegador aparecían el nombre del vendedor, su ubicación y, especialmente, el \*\*identificador interno de usuario en formato CUID2\*\*.”
>
> “Sin autenticarnos, pudimos enumerar cuatro usuarios y asociarlos con sus identificadores internos.”
>
> “Es importante matizar que mostrar el nombre de un vendedor o la ubicación general puede ser normal en un marketplace. El problema real es exponer el `userId` interno, porque puede reutilizarse para probar otros endpoints y construir ataques de autorización horizontal o IDOR.”
>
> “La solución es filtrar los objetos enviados al cliente, excluir identificadores internos y utilizar, cuando sea necesario, identificadores públicos no correlacionables. Además, cada endpoint debe comprobar en servidor que el usuario tiene autorización sobre el recurso solicitado.”

\---

## Slide 14 — H-04: configuración de Sentry expuesta  ⏱️ 10:35–11:25

> “También encontramos información de configuración de \*\*Sentry\*\* dentro de las etiquetas HTML públicas, incluyendo la clave pública y el identificador de organización.”
>
> “Aquí conviene hacer una precisión: una clave pública o DSN de Sentry no es equivalente a una contraseña secreta y, en determinadas integraciones de frontend, parte de esta información puede estar expuesta por diseño.”
>
> “El riesgo aparece por la cantidad de metadatos revelados y por la posibilidad de abusar del endpoint de ingestión para generar eventos falsos, contaminar la monitorización o facilitar reconocimiento dirigido.”
>
> “Por eso recomendamos reducir al mínimo la información enviada al cliente, mantener en servidor los valores que no sean estrictamente necesarios, aplicar filtros y límites de ingestión y revisar que Sentry no incluya datos sensibles en errores o trazas.”

\---

## Slide 15 — Otros hallazgos, controles y falsos positivos  ⏱️ 11:25–12:25

> “Los tres hallazgos restantes fueron de menor severidad.”
>
> “La etiqueta `robots=noindex` era incoherente con un entorno identificado como producción. No es una medida de seguridad: únicamente dificulta la indexación en buscadores.”
>
> “También se exponía el Project ID y la ruta pública de almacenamiento de Supabase. La URL pública puede ser necesaria para servir imágenes, por lo que el riesgo depende de la configuración real del bucket y de las políticas RLS. No confirmamos un acceso no autorizado a las tablas, por lo que la recomendación es \*\*verificar y, si fuera necesario, activar RLS\*\*, además de revisar permisos del bucket.”
>
> “Los productos eran accesibles sin login, pero esto puede ser intencional en un marketplace. Por eso se clasificó como informativo y no como un IDOR confirmado.”
>
> “También descartamos SSTI, SQL Injection y XSS reflejado. Los payloads se devolvían como texto o eran bloqueados por validaciones, sin ejecución. Documentar los falsos positivos demuestra que no aceptamos automáticamente las conclusiones de la herramienta.”

\---

## Slide 16 — Demostración de seguridad en directo  ⏱️ 12:25–14:00

> “Vamos a demostrar dos hallazgos de forma reproducible y no destructiva.”

### Demo 1 — Cabeceras ausentes

```bash
curl -sI https://bicipop.duckdns.org | grep -iE "x-frame|content-security|strict-transport|x-content-type"
```

> “El comando solicita únicamente las cabeceras HTTP y filtra las principales cabeceras de seguridad. Como vemos, no aparece ninguna coincidencia. La ausencia de salida es precisamente la evidencia.”

### Demo 2 — Identificadores internos en `/search`

```bash
curl -s "https://bicipop.duckdns.org/search?q=bici" | grep -o 'userId\[^,]\*'
```

> “En este caso consultamos una búsqueda pública y extraemos los campos `userId`. Los identificadores aparecen en la respuesta sin necesidad de iniciar sesión.”

> “Las dos pruebas son peticiones de lectura. No modifican información ni acceden a cuentas privadas.”

**Plan B:** llevar capturas de las dos ejecuciones y de la respuesta completa. Si el comando `grep` deja de coincidir por un cambio en el formato, mostrar la captura o buscar `userId` en el HTML guardado.

\---

## Slide 17 — Recomendaciones prioritarias  ⏱️ 14:00–14:55

> “Priorizamos las medidas atendiendo a su coste y a su impacto.”
>
> “Con carácter urgente, deben añadirse las cabeceras de seguridad en nginx y configurarse correctamente SPF y DMARC.”
>
> “En segundo lugar, debe eliminarse el `userId` interno de los payloads públicos y reducirse la información de Sentry expuesta al cliente.”
>
> “Por último, debe revisarse Supabase: comprobar que todas las tablas sensibles tengan RLS, validar los permisos del bucket y aplicar control de acceso en servidor a cualquier operación que reciba identificadores.”
>
> “La mayoría de estas medidas no requieren rediseñar la aplicación: son cambios de configuración y filtrado de datos con un impacto elevado en la seguridad.”

\---

## Slide 18 — Conclusiones  ⏱️ 14:55–15:35

> “Como conclusión, BiciPop parte de una base razonable: protege archivos sensibles, no expone rutas administrativas habituales y resistió las pruebas básicas de inyección que realizamos.”
>
> “Sin embargo, antes de un despliegue real deberían corregirse las cabeceras HTTP, la configuración de correo y la exposición de información interna.”
>
> “También aprendimos la importancia de saber dónde detener una prueba. Nuestro objetivo era demostrar cada riesgo con la evidencia mínima necesaria, sin modificar datos, comprometer cuentas ni provocar indisponibilidad.”

\---

## Slide 19 — Reflexión del equipo  ⏱️ 15:35–16:25

> “Como reflexión final, el proyecto nos aportó una visión completa del ciclo de una auditoría: definir el alcance, reconocer la aplicación, formular hipótesis, validar resultados, valorar el riesgo y proponer mitigaciones.”
>
> “Aprendimos a utilizar CAI y sus agentes, pero sobre todo aprendimos a cuestionar sus respuestas. No repetiríamos la dependencia inicial de modelos gratuitos ni aceptaríamos resultados sin evidencia reproducible.”
>
> “Lo que sí mantendríamos es el enfoque híbrido: utilizar automatización e inteligencia artificial para acelerar tareas, combinándolas con herramientas tradicionales y validación manual.”
>
> “El balance del equipo ha sido positivo porque la herramienta añadió velocidad, mientras que el criterio humano aportó fiabilidad.”

\---

## Slide 20 — Cierre y preguntas  ⏱️ 16:25–16:35

> “Muchas gracias por vuestra atención. Estamos disponibles para cualquier pregunta.”

*(Dejar la diapositiva final visible durante el turno de preguntas.)*

\---

# Versión recortada para una defensa estricta de 15 minutos

Para reducir aproximadamente un minuto y medio sin perder contenido esencial:

* Slide 2: limitar la presentación del equipo a 20 segundos.
* Slide 4: enseñar solo catálogo y una ficha de producto, sin realizar un alta completa.
* Slide 7: mencionar los cuatro agentes principales y dejar los agentes 15 y 14 para preguntas.
* Slide 8: resumir el proceso en 30 segundos.
* Slide 15: explicar los tres hallazgos adicionales en una sola frase y centrarse en los falsos positivos.
* Slide 19: dar una respuesta conjunta de 30 segundos.

\---

# Preguntas previsibles del tribunal

## ¿Por qué utilizar CAI y no limitarse a Burp Suite, Nmap o herramientas tradicionales?

> “Porque el objetivo diferencial del proyecto era evaluar un enfoque de pentest asistido por IA. CAI ayudó a automatizar reconocimiento, plantear hipótesis y coordinar agentes especializados. No sustituyó a las herramientas tradicionales: utilizamos `curl`, `dig`, Python y la terminal para validar los resultados.”

## ¿Qué es un payload RSC?

> “RSC significa React Server Components. Next.js envía al navegador datos serializados para construir o hidratar partes de la interfaz. El problema no es usar RSC, sino incluir en ese payload campos internos que el cliente no necesita, como el identificador interno del usuario.”

## ¿Confirmasteis realmente un IDOR?

> “Confirmamos la exposición de identificadores internos y la posibilidad de acceder públicamente a productos por ID. No confirmamos acceso a mensajes o perfiles privados de otros usuarios, porque no disponíamos de cuentas de prueba controladas para hacerlo sin afectar a usuarios reales. Por eso hablamos de un vector que habilita IDOR, no de compromiso completo de una cuenta.”

## ¿Que un producto sea público no es el funcionamiento normal de un marketplace?

> “Sí. Por eso el acceso público a productos se clasificó como informativo. Solo sería un IDOR si el identificador permitiera acceder a información u operaciones que deberían estar restringidas.”

## ¿La clave pública de Sentry no está diseñada para aparecer en el frontend?

> “En muchos casos, sí: el DSN público puede formar parte de una integración cliente. No lo tratamos como una contraseña. El riesgo está en la exposición adicional de metadatos, la posible contaminación del sistema con eventos falsos y la información que puedan revelar las trazas. La mitigación correcta es limitar datos, filtrar eventos y no exponer valores innecesarios.”

## ¿Por qué clasificasteis como crítico que falten cabeceras si no encontrasteis un XSS explotable?

> “La clasificación se aplicó al conjunto de cabeceras ausentes y a la falta total de endurecimiento del navegador. Reconocemos que, si se valorase cada cabecera de forma independiente y sin una vulnerabilidad encadenada, algunas organizaciones podrían asignarle una severidad menor. Lo importante es que la ausencia amplifica otros ataques y que su corrección tiene un coste muy bajo.”

## ¿La ausencia de SPF y DMARC garantiza que todos los correos falsos lleguen?

> “No. Los proveedores pueden aplicar filtros propios y reputación. Lo que significa es que el dominio no ofrece a los receptores una política fiable para autenticar el origen y rechazar suplantaciones, por lo que el riesgo aumenta considerablemente.”

## ¿Por qué recomendáis `v=spf1 -all`?

> “Es correcto únicamente si el dominio no envía correo legítimo. Si utiliza un proveedor, debe incluirse ese proveedor en SPF y finalizar con `-all`. DMARC debería desplegarse de forma progresiva, comenzando con monitorización y pasando después a cuarentena o rechazo.”

## ¿Exponer el Project ID de Supabase es una vulnerabilidad?

> “No necesariamente. La URL del proyecto y la anon key pueden ser públicas por diseño. El riesgo depende de las políticas RLS y de los permisos del bucket. En nuestro caso lo tratamos como exposición de arquitectura y recomendamos comprobar la configuración; no afirmamos haber obtenido acceso no autorizado a las tablas.”

## ¿Cómo evitasteis dañar la aplicación?

> “Aplicamos un enfoque no destructivo: peticiones de lectura, consultas DNS y payloads controlados. No realizamos fuerza bruta, denegación de servicio, modificación o borrado de datos, reverse shells ni exfiltración de información privada.”

## ¿Qué aportó realmente la IA?

> “Aceleró la enumeración y ayudó a generar líneas de investigación. Su principal limitación fue la fiabilidad: llegó a inventar rutas y resultados. El valor apareció al combinarla con validación manual y criterio técnico.”

## ¿Qué haríais en una segunda auditoría?

> “Repetiríamos las pruebas tras aplicar las remediaciones y ampliaríamos el alcance a controles de autorización horizontal con dos o más cuentas de prueba controladas. También revisaríamos sesiones, rate limiting, recuperación de contraseña, políticas RLS y gestión de dependencias.”

\---

# Checklist antes de la defensa

* Sustituir todos los campos **\[Nombre del compañero/a]**.
* Confirmar qué persona presenta cada bloque.
* Ensayar la presentación completa con cronómetro al menos tres veces.
* Probar la aplicación y los dos comandos la víspera y el mismo día.
* Preparar capturas de pantalla como plan alternativo.
* Guardar una copia del PowerPoint en PDF y llevarla también en USB.
* Comprobar que el equipo de la sala muestra correctamente fuentes, código y vídeos.
* Mantener abierto el informe completo por si solicitan evidencias adicionales.

\---

# Inconsistencia que conviene corregir antes de presentar

La numeración de los hallazgos no coincide entre el PowerPoint y el informe escrito:

* En el **PowerPoint**, el email spoofing aparece como **H-02** y Sentry como **H-03**.
* En el **informe**, Sentry aparece como **H-02** y el email spoofing como **H-03**.

El guion sigue la numeración del PowerPoint para que coincida con lo que verá el tribunal. Lo recomendable es unificar ambos documentos antes de la defensa.

