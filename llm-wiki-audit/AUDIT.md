# FASE 0: Auditoria del conocimiento y propuesta de LLM Wiki

Fecha de corte: 2026-11-14. Estado: AUDITORIA PROGRESIVA ABIERTA. No autoriza la FASE 1.

Este informe es una evaluacion y propuesta, no una wiki implementada ni una certificacion del banco. Alcance detallado: [INVENTORY.md](INVENTORY.md). Resultados reproducibles y limites: [CHECKS.md](CHECKS.md).

## 01. Executive Summary

**Recomendacion:** conservar las fuentes y construir, solo tras autorizacion, una capa pequena de Markdown con afirmaciones trazables, relaciones explicitas y estado de certeza por afirmacion. No empezar por RAG, una base de datos ni una migracion masiva.

Se inventariaron 434 archivos de contenido, incluidos 27 Markdown, 254 XML y 38 Python. Se leyeron los 27 Markdown del repositorio, se analizaron estructuralmente los 254 XML, se contrastaron 72 definiciones SYNC y se revisaron rutas seleccionadas de codigo, tests, registros y curso externo. No equivale a haber validado semanticamente todos los protocolos ni todas las ejecuciones.

El material tiene valor: explica la separacion entre anfitrion, DLL, VarDB, configuracion y tests; contiene configuraciones reales y procedimientos reutilizables. Sin embargo, la repeticion de ejemplos incorrectos crea una apariencia de consenso. El curso incorpora correcciones ausentes en otras guias, pero tambien conserva contradicciones internas. La hipotesis inicial de propagacion incompleta queda respaldada; no existe un documento enteramente canonico demostrado.

Los principales riesgos son operativos: filtros que omiten canales, XML presentados como ejemplos completos con dependencias ausentes, sintaxis ilustrativa presentada como real, conclusiones de diagnostico demasiado fuertes y ausencia de trazabilidad entre binario, configuracion y registro. Se reprodujeron la perdida de canales del generador y la devolucion de un valor no estabilizado por el lector VarDB, sin abrir memoria compartida ni arrancar simuladores.

El conocimiento contextual adicional incluye el puente TrainSignalFbck, el orden de creacion de STR, diferencias entre banco aislado e integrado, restricciones del lector y una rama externa de endurecimiento del ciclo de vida. Parte ya esta documentada; parte procede solo del contexto historico y necesita recuperar su evidencia primaria.

**No se declara concluida la FASE 0:** quedan las comprobaciones P01-P08 del inventario. Las decisiones humanas pendientes son seleccionar variantes canonicas, identificar responsables y fuentes normativas, aprobar la politica de distribucion y delimitar el piloto. No se modificaron fuentes para resolver los hallazgos.

## 02. Repository Inventory

El inventario excluye entornos virtuales, Git interno, caches y este directorio. Incluye nombres de tres archivos de claves, pero no se leyo ni se reproduce su contenido. Las cantidades corresponden a archivos, no a simuladores desplegados.

| Clase | Cantidad | Valor y limitacion |
|---|---:|---|
| Markdown | 27 | Referencia, instruccion para agentes, curso, procedimientos e informes; autoridad desigual |
| XML | 254 | Configuracion declarada; parseo correcto no demuestra aceptacion por GenericSimulator |
| TXT | 55 | Mezcla de lanzadores CONNECTION, indices, analisis y registros |
| Python | 38 | Herramientas, pruebas, extractores y migraciones; varios scripts escriben al ejecutarse |
| MAMUT | 9 | Casos completos y ficheros comunes; dependencias externas |
| C++ / cabeceras | 4 / 3 | Implementacion local del anfitrion y parser, no de todo el ecosistema |
| EXE / DLL | 5 / 2 | Artefactos sin correspondencia de compilacion demostrada |
| Otros | 37 | Incluyen 9 DAT, 7 JSON, 6 LOG, 3 KEY, 2 BAT, XLSX, PDF y metadatos de build |

El estado Git inicial mostraba 43 entradas no seguidas, muchas de ellas directorios completos de conocimiento. No deben confundirse con 43 archivos. Gran parte de la documentacion no tiene historial Git disponible. Las dos referencias homonimas y las dos copias de TP2000 tienen hashes diferentes; el diff del test muestra cabecera adicional y tres requisitos JRU adicionales, no una secuencia de pasos diferente.

Existen dos corpus de configuracion: los 18 dominios del paquete y la carpeta plana del generador. Dieciseis archivos XML con nombre coincidente difieren por hash. Esto no prueba dieciseis diferencias semanticas; si impide tratarlos como copias intercambiables sin comparacion.

## 03. Knowledge Map

| Dominio | Conocimiento util | Fuente de contraste | Madurez actual |
|---|---|---|---|
| Ciclo de vida | CONNECTION, carga DLL, estados, supervision y stop | [Src/simul_handler.cpp](../Src/simul_handler.cpp), [Src/simul_executer.cpp](../Src/simul_executer.cpp) | Implementacion local examinada; equivalencia con binarios UNKNOWN |
| Configuracion | CNX, SYNC, INTERFACE, objetos serializables y LOGIC | XML y libro XLSX | Buena base primaria; ejemplos simplificados no siempre autocontenidos |
| VarDB | Nombres, tipos, normal/forced, memoria compartida | XML y [tools/vardb_reader.py](../tools/vardb_reader.py) | Lector concreto examinado; ABI universal y atomicidad no demostrados |
| Red | Canales, direcciones, puertos y filtros | CNX de ambos corpus | Endpoints declarados VERIFIED; rutas completas por gateways INFERRED/UNKNOWN |
| MAMUT | Estimulos, expresiones, tiempos, threads, conectores | Tests y comunes; manual referido en el curso | Test TP2000 contrastado; revision oficial del lenguaje pendiente |
| Movimiento | Perfil, STR, puente a ENC/RAD, arranque | Curso, guia de herramientas, lanzadores e historia | Mezcla de codigo disponible, ejemplo e historial de banco |
| Diagnostico | Separar estimulo, copia, envio, recepcion y comprobacion | Logs, XML y herramientas | Util si conserva alternativas; no apto para atribucion automatica de culpa |
| JRU / Q_LRBG | Cambios de layout, condicionales y variantes | Informes, SYNC, scripts y protocolos | Conflicto de versiones y validacion funcional pendiente |
| Pedagogia/agentes | Curriculo, secuencia de lectura y ejercicios | Paquete docente y curso | Buena organizacion; autoridad excesiva y errores repetidos |

No se asigna una unica etiqueta de certeza a una fila completa. Tampoco se interpreta un acronimo a partir de su nombre: las expansiones de BTM, MOD, RTM o MVB deben tener fuente de dominio.

## 04. Relationship Analysis

La unidad de conocimiento debe ser una relacion comprobable, no una lista de documentos que se citan circularmente.

| Relacion | Evidencia y alcance |
|---|---|
| SimulExecuter **lee** CONNECTION y **carga** DLL | Parser y handler locales; no es un GenericSimulator.exe separado |
| CONNECTION **selecciona** CNX y SYNC | CONNSTR/SYNCSTR; resolver rutas respecto al entorno de lanzamiento, no asumir la carpeta del informe |
| SYNC **selecciona** protocolo, INTERFACE y logica | Atributos actuales; JRU selecciona test2 en el paquete y protocol_JRU en el generador |
| LOGIC **asocia** comprobacion y accion | Referencias por ident; las acciones pueden copiar, recibir o enviar |
| OBJSER **referencia** objetos que **serializan** campos | El recorrido del contenedor determina orden y bits; declarar una VAR no implica enviarla |
| MAMUT **escribe/consulta** variables y **coordina** threads | Tests; el conector define el acceso, que no siempre es memoria local |
| TrainSignalFbck **copia** STR::SPEED hacia ENC/RAD | Curso y guia, respaldados por contexto historico; falta asociar fuentes externas y ejecucion a hashes |
| Log/captura **observa** un resultado | No prueba por si solo causa, cumplimiento normativo ni que corresponde al XML actual |
| Guia **deriva de** una fuente / **contradice** otra | Debe conservar la version y el fragmento preciso, no aumentar confianza por repeticion |

Ejemplo de recorrido candidato para el piloto: perfil -> TrainSimulPrf -> STR::SPEED -> TrainSignalFbck -> ENC1::ENC1_SPEED -> logica normal/forced -> ENC1::ENC_1_SPEED -> contenedor ENC -> UDP destino 51301. El tramo observado historicamente termina en la captura; recepcion y procesamiento por EVC requieren evidencia adicional.

Las relaciones hoy debiles son: requisito -> clausula normativa; binario -> commit/build; captura -> comando/configuracion; ejemplo -> banco compatible; error -> prueba que discrimina causas. El enlace EBAL -> BTM tiene endpoints compatibles en el paquete; BTM -> BLT no se demuestra con 51601 frente a 51611. No se rellena ese salto con un gateway supuesto.

## 05. Knowledge Discrepancies

Prioridades: P0 bloquea confianza o ejecucion del material afectado; P1 puede inducir diagnostico erroneo; P2 afecta mantenimiento o alcance. VERIFIED significa que se comprobo el contraste indicado, no que se certifico todo el subsistema. OBSOLETE se propone solo para una afirmacion reemplazada en un alcance especifico; no se marca un archivo entero por ser antiguo.

| ID | Prioridad | Contraste y estado | Tratamiento propuesto, sin aplicarlo |
|---|---|---|---|
| D01 | P0 | VERIFIED: [SIMULATOR_AGENT_REFERENCE.md](../SIMULATOR_AGENT_REFERENCE.md) describe CHANNEL/FIELD/SYNC_CONFIGURATION/STATES frente a CNX/VAR/OBJSER/SYNC/LOGIC de los ejemplos actuales | Clasificar esos esquemas como ilustrativos no ejecutables; no usarlos para generar XML real |
| D02 | P0 | VERIFIED: [AGENT_PACKAGE/di_config/logic_config_DI.xml](../AGENT_PACKAGE/di_config/logic_config_DI.xml) usa cnt_DISIM_SET_DIGINPUT_VALUES y cnt_DISIM_STATUS, ausentes del protocolo que selecciona su SYNC | Retirar la garantia documental de ejemplo completo; verificar origen de la simplificacion antes de repararla |
| D03 | P0 | VERIFIED: SYNC EBAL_X21 referencia un archivo no adyacente; SYNC JRU del paquete selecciona test2, donde faltan 11 destinos unicos usados por la logica | Documentar dependencias/variantes; UNKNOWN resolucion externa y resultado de carga. No cambiar automaticamente a otra variante |
| D04 | P0 | VERIFIED: [wireshark_filter_generator/parser.py](../wireshark_filter_generator/parser.py) conserva solo una entrada y una salida por instancia; reproduccion pierde ATO 60005, RAD 51221 y tres destinos EBAL | No confiar en BOTH como captura completa; futura correccion multicanal y tests de regresion |
| D05 | P1 | VERIFIED: RX/TX del generador son udp.port sin direccion; DMI inout da 50311 para ambos y no aísla su 50511 local | Separar puerto local/remoto, IP, transporte y sentido; no deducir gateway solo por puerto compartido |
| D06 | P1 | VERIFIED: [tools/vardb_reader.py](../tools/vardb_reader.py) devuelve 4.0/OK tras cuatro bloques sinteticos diferentes; [tools/USER_GUIDE.md](../tools/USER_GUIDE.md) promete consistencia mas fuerte | Expresar best effort y estado inestable; comprobar limites del buffer/ABI antes de ampliar garantias |
| D07 | P1 | VERIFIED estructural: ENC tiene 42 bytes de campos seleccionados, no 32; logica solo envia. Compresion 42->25 y recepcion obligatoria repetidas en guias no estan respaldadas por este camino | Proponer OBSOLETE para esas afirmaciones en este ENC; no universalizar a todos los transportes ni versiones |
| D08 | P1 | VERIFIED por struct: 150.5 big-endian es 43 16 80 00; 00 80 16 43 little-endian tambien es 150.5. Curso 2.1 conserva bytes incorrectos y 2.6 afirma lo contrario para little-endian | Corregir cada ejemplo y distinguir offset de payload de offset de trama dependiente de encapsulacion |
| D09 | P1 | VERIFIED: [AGENT_PACKAGE/MAMUT_INTEGRATION_GUIDE.md](../AGENT_PACKAGE/MAMUT_INTEGRATION_GUIDE.md) ensena READ y milisegundos, mientras curso corregido y tests usan expresiones y constantes temporales distintas | Revision contra manual oficial pendiente; no presentar ejemplos con READ/WAIT 50ms como ejecutables |
| D10 | P1 | VERIFIED: [AGENT_PACKAGE/REAL_TEST_ANALYSIS.md](../AGENT_PACKAGE/REAL_TEST_ANALYSIS.md) habla de 8 pasos y 2-3 segundos; test tiene 36 y espera 60; registro llega aproximadamente a 77.55 | Rehacer analisis desde version concreta; no interpretar desk B como puerta |
| D11 | P1 | VERIFIED: [AGENT_PACKAGE/EXAMPLES/README_EJEMPLOS.md](../AGENT_PACKAGE/EXAMPLES/README_EJEMPLOS.md) propone identificacion del conductor como precondicion del paso 9; en [TP_AppEVC_2000.mmts](../TP_AppEVC_2000.mmts#L2867) se termina 9 antes de iniciar 13 | Preservar utilidad del diagnostico temporal, pero no esa dependencia inventada |
| D12 | P1 | VERIFIED: comprobacion DMI acepta un buffer coincidente con OR, no exige los cinco. Curso 11 lo explica; parte del analisis antiguo no | Asociar la explicacion al CHK y a las macros de limpieza; no garantizar frescura sin examinarlas |
| D13 | P1 | VERIFIED: libro XLSX distingue size en bits; DI declara Boolean1 size=1. Las cuentas de 160 bytes suman valores y estados como si fuesen un mensaje y usan ancho de memoria | No dar tamano total de DI hasta seleccionar y recorrer un contenedor valido; 32 bits no son 32 bytes |
| D14 | P1 | VERIFIED: la guia Q_LRBG reconoce limites y migracion parcial; FASE_4A anuncia conformidad con pruebas funcionales pendientes. Su script usa COND var_id/op=EQ frente a idserobj/operator=equal del libro | Separar parseo XML, compatibilidad de parser y conformidad normativa; no promover variante VARCOND a validada |
| D15 | P1 | VERIFIED metodologico: [.analyze_reachability_v2.py](../.analyze_reachability_v2.py) usa INTERFACE como raiz, recorre contencion y omite referencias sin id en su mapa | 100% alcanzable no acredita uso en ejecucion ni ausencia de referencias rotas; no fundamentar eliminaciones/deduplicacion en ello |
| D16 | P1 | VERIFIED documental: [AGENT_PACKAGE/DEBUG_WORKFLOW.md](../AGENT_PACKAGE/DEBUG_WORKFLOW.md) convierte falta de respuesta, LISTENING o checklist completo en atribucion al EVC | Cambiar por hipotesis con comprobaciones discriminantes; UDP no tiene el estado TCP LISTENING |
| D17 | P1 | VERIFIED: [AGENT_PACKAGE/network_visualizer.py](../AGENT_PACKAGE/network_visualizer.py) usa clave familia+ident, sin instancia/archivo; canales homonimos de distintas instancias se sobrescriben | No tratar diagrama generado como inventario exhaustivo; conservar identidad de instancia y canal |
| D18 | P1 | VERIFIED: [tools/stop_demo.bat](../tools/stop_demo.bat) fuerza cierre por nombre de imagen de todos los bancos; start usa esperas fijas sin readiness | Mantener advertencia y proponer control por procesos propios. No ejecutar estos comandos como diagnostico de lectura |
| D19 | P1 | VERIFIED para fuente local: salida del supervisor por error y destructor no demuestran parada cooperativa completa. Historia de endurecimiento se refiere a otro checkout | Versionar por separado fuente local, rama externa y binarios; UNKNOWN equivalencia entre ellos |
| D20 | P2 | VERIFIED: las dos copias TP2000 difieren en cabecera y REQ; la anidada anade 3 requisitos JRU | Elegir autoridad con responsable; no declarar 27 requisitos para la copia raiz, que contiene 24 |
| D21 | P1 | VERIFIED: el curso 11 dice que LOCATION no cambia; el registro existente muestra 1.041898 y 1.070639 al apagar | Explicar que el perfil espera 75 s y el test puede superarlos; no inferir inmovilidad de todo el ensayo |
| D22 | P1 | UNKNOWN: clausulas Subset, causas internas EVC, ventanas normativas y ciertos nombres ilustrativos no se vinculan a una especificacion versionada | Obtener documento/edicion/clausula; las constantes del test no son por si solas requisitos normativos |
| D23 | P2 | VERIFIED: instrucciones docentes hacen del paquete la unica verdad y a veces excluyen runtime, mientras los procedimientos requieren runtime/red | Sustituir precedencia absoluta por politica de evidencia; los registros de aprendizaje no son confirmaciones independientes |
| D24 | P2 | VERIFIED: export_lua devuelve una tabla pero no registra botones; la descripcion promete botones UI | Alinear alcance documentado con exportacion real; integracion GUI UNKNOWN, no probada aqui |

Otros puntos que requieren acotacion: valores iguales en STR y DMI no prueban toda la cadena; mapping existente no prueba publicador vivo; ausencia de ICMP no prueba consumo por EVC; falta de una variable *_FORCED no prueba ausencia del mecanismo FORCE del conector. No basta una fecha reciente para resolver ninguno de estos conflictos.

## 06. Contextual Knowledge Gaps

Se utilizo el resumen conversacional disponible y memorias del repositorio sobre MAMUT, exploracion/endurecimiento y migracion del generador. No se reconstruyo todo el historial de sesiones ni se consulto de nuevo cada captura citada en el pasado. Una memoria puede orientar una busqueda, pero no sustituye la prueba.

| Contexto | Ya persistido | Lo que falta para una ficha fiable |
|---|---|---|
| TrainSignalFbck une STR con ENC/RAD | Curso 9 y guia tools | Fuente externa versionada, variables exactas y pruebas de conversion/unidades |
| Feedback antes de perfil: STR 100000 frente a 4096 bytes | Curso, guia y start_demo | Vincular error historico, binarios y comando; no tratar el orden como regla universal de cualquier version |
| SIM_START_SYNC en banco integrado, sin el flag en demo aislada | Curso y lanzador | Matriz de escenarios y propietario de SIM_START |
| Captura ENC de 42 bytes, ~31 ms, destino 51301 | Guia y contexto historico | Captura original con hash, interfaz, filtros y version del emisor |
| Lector solo escalar y lectura sin crear mappings | Codigo y guia | Evidencia ABI, limites de consistencia, calidad visible y pruebas negativas |
| Endurecimiento de stop/join y limite de nombre de pipe | Memoria de otro checkout | Diff/commit, pruebas originales y correspondencia de binarios; UNKNOWN en esta auditoria |
| Curso corregido con ConnNoSync, START/JOIN y 36 pasos | Texto actual examinado | Registro de correcciones y propagacion a referencias/ejercicios |
| Error GENERAL_MESSAGE_NID_LRBG | Log del directorio TP2000 | Vinculo a protocolo realmente cargado, captura y diagnostico aprobado |

No se atribuyen explicaciones tecnicas al experto humano cuando solo consta un resumen del agente. Una futura entrevista debe preguntar por decisiones, invariantes y excepciones; cada respuesta quedaria como testimonio fechado, no como VERIFIED automaticamente.

## 07. Documentation Quality Assessment

Evaluacion cualitativa, sin una puntuacion numerica que sugiera precision inexistente:

| Dimension | Valoracion | Motivo |
|---|---|---|
| Cobertura tematica | Amplia | Arquitectura, XML, tests, red, movimiento, docencia y JRU |
| Exactitud | Desigual | Fuentes reales conviven con layouts, nombres y dependencias inventados |
| Trazabilidad | Baja | Faltan hashes, revisiones de binarios, localizadores de clausula y pruebas asociadas |
| Navegacion | Media | Indices y guias ayudan; dos referencias homonimas y copias externas confunden |
| Reproducibilidad | Baja-media | Comandos utiles, pero dependencias externas, scripts mutadores y versiones ambiguas |
| Seguridad operativa | Insuficiente | Paradas globales, claves y credenciales de ejemplo mezcladas con material distribuible |
| Separacion didactica/evidencia | Baja | Ejemplos se llaman reales o validados sin artefacto correspondiente |
| Portabilidad entre agentes | Baja-media | Markdown portable; instrucciones de rol, rutas personales y lecturas exhaustivas lo dificultan |

Las guias de logica/protocolo son candidatas utiles para derivacion desde el libro, pero este tambien incluye comentarios tentativos y no especifica toda la semantica de get_last_msg. El curso es una buena ruta pedagogica, no una fuente normativa. Q_LRBG_MIGRATION tiene mejor tratamiento de incertidumbre que los informes de optimizacion. Ninguna de estas valoraciones autoriza descartar originales.

## 08. Diagnostic Capabilities

Una wiki fiable deberia responder con la siguiente forma: observacion -> posibles causas -> comprobacion de lectura -> resultado esperado -> limite de conclusion -> siguiente decision.

| Pregunta de evaluacion | Capacidad actual | Respuesta exigible al piloto |
|---|---|---|
| Que carga SimulExecuter y donde? | Alta para fuente local | Citar carga DLL/CONNECTION; no inventar proceso GenericSimulator.exe |
| Por que ENC no recibe respuestas? | Alta para esta logica | Explicar ausencia de accion de recepcion; no recomendar anadirla automaticamente |
| Por que STR cambia pero ENC no? | Media | Separar publicador, puente, copia normal/forced y envio; comprobar cada punto |
| El filtro BOTH muestra todo ATO/BTM? | Actualmente no | Identificar canales perdidos y negarse a atribuir ausencia de paquetes al banco |
| El paso 9 necesita el 13? | Alta | Mostrar START/JOIN reales y rechazar la precondicion propuesta |
| El paquete JRU cumple la norma? | Insuficiente | Solicitar version normativa y evidencia; distinguir XML parseable de layout correcto |
| Hay una lectura VarDB atomica? | No demostrado | Explicar limitacion best effort, no confundir quality=OK con estabilidad |
| Se puede reutilizar un contenedor JRU? | Insuficiente | Analizar identidad/estado observable por mensaje antes de deduplicar |
| Que binario implementa stop cooperativo? | UNKNOWN | Pedir procedencia de build; no transferir conclusiones de otra rama |

No se midio un benchmark real de respuestas de varios modelos en esta fase. Se propone evaluar exactitud, calidad de citas, manejo de contradicciones, abstencion ante UNKNOWN y ausencia de acciones no autorizadas; no premiar solo fluidez o cantidad de contexto.

## 09. Proposed LLM Wiki Architecture

**Arquitectura recomendada: documentos Markdown enlazados con metadatos YAML pequenos, fuentes inmutadas y un indice neutral.** Es una propuesta; no se crean esas carpetas ni paginas.

| Area propuesta | Contenido | Regla de tamano/propiedad |
|---|---|---|
| Entrada | Indice, alcance, como elegir escenario y politica de evidencia | Una entrada corta; no obligar a leer todo el corpus |
| Componentes | SimulExecuter, DLL, MAMUT, VarDB, TrainSimulPrf, Feedback | Responsabilidad, contratos, limites y enlaces; evitar una pagina por cada variable |
| Interfaces/configuracion | CNX/SYNC, serializacion, ENC/DI/JRU, nombres y unidades | Una pagina por contrato coherente; separar variantes incompatibles |
| Flujos | Arranque aislado, integrado, velocidad, comprobacion DMI, parada | Precondiciones, pasos, evidencias y efectos secundarios |
| Diagnostico | Sintoma concreto con hipotesis alternativas y checks | No duplicar arquitectura; enlazarla |
| Evidencias/fuentes | Registro de procedencia y ejecuciones aprobadas | Referenciar originales; no copiar secretos ni binarios indiscriminadamente |
| Decisiones/contradicciones | Resoluciones aprobadas y asuntos abiertos | Crear registro solo cuando exista una decision o conflicto real |

Cada pagina tendria: proposito y escenario; hechos con certeza/fuente; limites; relaciones; comprobaciones; cuestiones abiertas. Los ejemplos ejecutables y el pseudocodigo estarian rotulados de forma distinta. Las tablas de puertos se derivarian del corpus seleccionado y conservarian IP, transporte, instancia, canal, extremo local y remoto.

Las relaciones se representan por enlaces Markdown y predicados como loads, configured_by, publishes, copies, serializes, observed_in, contradicts y supersedes. No requieren un grafo de infraestructura. Un indice inverso de fuentes afectadas puede ser una tabla mantenida o un chequeo futuro, no una base de datos inicial.

El contenido neutral funcionaria con lectura de archivos y busqueda textual. Adaptadores pequenos para Copilot, Claude u otro agente serian opcionales y solo apuntarian a la misma entrada; no contendrian otra copia de los hechos. La memoria del proveedor nunca seria fuente canonica ni requisito de acceso.

## 10. Metadata and Traceability Model

| Certeza | Uso | Condicion minima |
|---|---|---|
| VERIFIED | Afirmacion comprobada dentro de un alcance | Fuente primaria/localizador y metodo; indicar si fue lectura, prueba sintetica o banco real |
| INFERRED | Conclusion razonada no observada directamente | Premisas enlazadas y razonamiento falsable |
| HYPOTHESIS | Causa o comportamiento por comprobar | Prediccion y prueba discriminante |
| UNKNOWN | No hay evidencia suficiente o falta resolver conflicto | Dato necesario, responsable o bloqueo |
| OBSOLETE | Afirmacion antes aplicable y ya sustituida para ese alcance | Sustitucion, version y motivo; conservar historia |

Separar certeza de ciclo editorial: draft/reviewed/approved/retired. Una pagina approved puede contener un UNKNOWN explicitamente aceptado. Una afirmacion falsa desde el origen no pasa a ser historia valida al etiquetarla OBSOLETE: debe figurar como refutada en el registro de contradicciones.

Campos propuestos por pagina: id estable, titulo, tipo, dominio, escenario/version aplicable, estado editorial, responsable, fecha de revision y relaciones. Por afirmacion: claim_id, texto, certainty, scope, source_ids, locator, metodo de verificacion, evidence_ids, fecha, contradicciones y pregunta pendiente.

Por fuente: source_id, clase (codigo/configuracion/documento/test/testimonio/log), repositorio o coleccion, ruta relativa, commit cuando exista, SHA-256 cuando sea necesario, revision declarada y restricciones de distribucion. Lineas son localizadores auxiliares; complementar con simbolo, XPath/ident o seccion para resistir cambios. Para XLSX usar hoja y celda/fila; para captura, numero de paquete y offset de payload.

Por ejecucion: comando saneado, directorio, escenario, versiones/hashes de ejecutables y configuraciones, hora, OS/interprete, proceso observado, filtros/interfaz, resultado bruto y limites. Evitar rutas de usuario como identidad portable. Las credenciales se redactan; un hash no sustituye autorizacion para distribuir el contenido.

Ejemplo conceptual, no pagina creada: C-ENC-001 = "cnt_ENCBOARD_MSG contiene 336 bits en este XML"; VERIFIED por recorrido de OBJSER y suma de size; fuente protocol_config_ENC; alcance hash concreto. Otra afirmacion, "se observaron 42 bytes UDP", necesita su propia captura y no hereda la verificacion estructural de C-ENC-001.

## 11. Maintenance Strategy

1. Registrar cambios de fuente antes de actualizar explicaciones: corpus, revision y responsable.
2. Identificar las afirmaciones derivadas afectadas mediante source_id; marcar revision necesaria, sin borrarlas automaticamente.
3. Revalidar solo el alcance afectado: referencias/layout para XML, sintaxis y secuencia para MAMUT, evidencia real si se afirma comportamiento runtime.
4. Resolver contradicciones con una decision registrada: evidencia, versiones compatibles, afirmacion reemplazada y razones. No votar entre documentos ni preferir siempre el mas reciente.
5. Revisar conjuntamente fuente y derivacion; el experto de dominio valida normativa y supuestos del banco, no solo estilo Markdown.
6. Comprobar enlaces, IDs, metadatos, referencias de configuracion y ejemplos en un entorno aislado. No ejecutar launchers ni scripts de migracion como parte de una validacion documental rutinaria.
7. Versionar entregables aprobados con las fuentes permitidas; mantener artefactos sensibles fuera del paquete distribuible.

Disparadores de revision: cambio de XML/binario, nuevo escenario, fallo reproducible, correccion experta o incompatibilidad entre agentes. La periodicidad administrativa no reemplaza estos disparadores. Un cambio de contenido de captura o config invalida sus afirmaciones dependientes hasta reexaminarlo.

No se propone automatizacion de edicion autonoma como punto de partida. Primero deben existir reglas y revisores; despues pueden automatizarse inventario, enlaces, diferencias y avisos, sin promover afirmaciones a VERIFIED solo porque un script termina con codigo cero.

## 12. Implementation Roadmap

| Etapa propuesta | Entregable | Puerta de salida |
|---|---|---|
| 0A, este corte | Auditoria, inventario, contradicciones y checks | Entrega progresiva, no cierre de FASE 0 |
| 0B, completar auditoria | Resolver cobertura P01-P08 y recuperar evidencia prioritaria | Cada area relevante examinada o alcance/exclusion expresamente acordado; bloqueos visibles |
| Decision humana | Corpus y responsables canonicos; politica de acceso; alcance piloto | Aprobacion explicita para FASE 1 |
| 1, piloto ENC | Entrada neutral, contrato ENC, flujo de velocidad y diagnostico con evidencias | Responder preguntas de la seccion 08 sin inventar recepcion, compresion o causalidad |
| 2, extension MAMUT/DI | Test TP2000 versionado, tiempos, buffers y conectores | Ejemplos validados y sin referencias ausentes en el conjunto aprobado |
| 3, JRU/red compleja | Variantes, requisitos, rutas y decisiones de layout | Revision experta/normativa y evidencia runtime autorizada |
| 4, mantenimiento | Chequeos ligeros y politica de actualizacion | Cambio de fuente detecta derivaciones afectadas; portabilidad comprobada en mas de un agente |

Orden recomendado de saneamiento futuro: D01-D05 y D09-D15 antes de convertir guias en instrucciones ejecutables; despues robustez de herramientas y operacion; finalmente ampliacion pedagogica. No se estiman jornadas sin responsables, acceso a fuentes y definicion de completitud.

## 13. Risks and Recommendations

**Riesgo principal:** una wiki bien presentada puede amplificar errores existentes si ingiere todo con igual autoridad. Mitigacion: trazabilidad por afirmacion, escenarios/versiones y abstencion explicita.

- No eliminar fuentes duplicadas ni variantes JRU antes de conocer su funcion y consumidores; compartir un contenedor puede compartir estado observable, no solo ahorrar texto.
- No distribuir claves, credenciales, registros o binarios propietarios por defecto. Revisar permisos y sanitizacion antes de cualquier servicio externo o paquete para agentes.
- No confundir parseo XML, tests unitarios de utilidades, registro de un ensayo y conformidad normativa. Son capas de evidencia diferentes.
- No promover una topologia inferida por puertos a arquitectura comprobada; preservar los canales omitidos y los saltos desconocidos.
- No confundir nombres de versiones como original/ok/last/test2 con validacion; registrar procedencia y la seleccion efectiva en SYNC.
- No reiniciar el banco para completar esta auditoria ni ejecutar scripts de migracion. Las pruebas runtime adicionales requieren alcance y autorizacion propios.

Validacion humana requerida: autoridad entre copias TP2000; origen de los XML simplificados DI/ENC; variante JRU realmente soportada; correspondencia de builds; documento normativo/edicion; semantica de conectores y gateways; responsables de revision; politica de informacion sensible; cierre de alcance de FASE 0 y posterior autorizacion de FASE 1.

Limitaciones explicitas: CodeGraph no tenia proyecto cargado y no se creo un indice; no se instalo nada; no se ejecuto el banco ni se certifico la DLL; no se evaluo exhaustivamente cada protocolo ni cada archivo auxiliar. El inventario conserva estos pendientes para impedir que este informe se use como declaracion de auditoria terminada.