# Inventario y cobertura de FASE 0

Fecha de corte: 2026-11-14. Este archivo documenta el alcance de [AUDIT.md](AUDIT.md), no un catalogo de paginas de wiki ya creadas.

## Criterios de cobertura

- Lectura: contenido examinado; no significa que todas sus afirmaciones sean correctas.
- Contraste focal: solo las rutas/secciones necesarias para verificar afirmaciones concretas.
- Estructural: parseo y consultas de elementos/referencias; no ejecucion ni validacion de esquema propietario.
- Inventariado: existencia/clase/tamano o hash; contenido aun no evaluado completamente.
- Excluido: caches, entornos, Git interno y contenido criptografico. Los informes de auditoria no cuentan como fuentes originales.

## Documentos Markdown del repositorio

Los 27 documentos siguientes se leyeron. La columna de riesgo distingue relacion/duplicacion de autoridad. Relevancia A = imprescindible para el nucleo; M = apoyo especializado.

| Documento | Tipo, proposito y relaciones | Relevancia y riesgo |
|---|---|---|
| [mamut.md](../mamut.md) | Instruccion de interpretacion de tests; enlaza paradigma de estimulo y VarDB | A; absolutismos y nombres no contrastados |
| [SIMULATOR_AGENT_REFERENCE.md](../SIMULATOR_AGENT_REFERENCE.md) | Referencia general antigua; flujo txt->DLL->XML | A; esquemas ilustrativos incompatibles con ejemplos actuales, D01 |
| [AGENT_PACKAGE/SIMULATOR_AGENT_REFERENCE.md](../AGENT_PACKAGE/SIMULATOR_AGENT_REFERENCE.md) | Referencia ENC/DI y serializacion; deriva de configs | A; no equivalente a la anterior; tamanos/compresion incorrectos |
| [AGENT_PACKAGE/README.md](../AGENT_PACKAGE/README.md) | Entrada del paquete y matriz de preguntas | A; garantias de cobertura/validez mas fuertes que sus fuentes |
| [AGENT_PACKAGE/AGENT_PROMPT.md](../AGENT_PACKAGE/AGENT_PROMPT.md) | Itinerario de lectura y adopcion de rol | M; rutas personales y gran carga inicial; no verificador |
| [AGENT_PACKAGE/AGENT_ROLE.md](../AGENT_PACKAGE/AGENT_ROLE.md) | Limites del asesor y ejemplos de validacion | A; excluye runtime pero pretende diagnosticar; ejemplo ENC32 |
| [AGENT_PACKAGE/QUICK_REFERENCE.md](../AGENT_PACKAGE/QUICK_REFERENCE.md) | Comandos y resumen operativo | A; duplica errores y menciona referencia de topologia no presente |
| [AGENT_PACKAGE/DEBUG_WORKFLOW.md](../AGENT_PACKAGE/DEBUG_WORKFLOW.md) | Flujo de fallos MAMUT/config/red/EVC | A; util como estructura, conclusiones causales excesivas |
| [AGENT_PACKAGE/MAMUT_INTEGRATION_GUIDE.md](../AGENT_PACKAGE/MAMUT_INTEGRATION_GUIDE.md) | Lenguaje y relacion con simuladores | A; READ/tiempos y ejemplos requieren reconciliacion |
| [AGENT_PACKAGE/REAL_TEST_ANALYSIS.md](../AGENT_PACKAGE/REAL_TEST_ANALYSIS.md) | Interpretacion TP2000 y escenarios de fallo | A; contradice secuencia, duracion y buffers actuales |
| [AGENT_PACKAGE/EXAMPLES/README_EJEMPLOS.md](../AGENT_PACKAGE/EXAMPLES/README_EJEMPLOS.md) | Casos pedagogicos de diagnostico | M; separacion temporal util; precondicion paso13->9 incorrecta |
| [AGENT_PACKAGE/NETWORK_ARCHITECTURE.md](../AGENT_PACKAGE/NETWORK_ARCHITECTURE.md) | Clases de conexion y procedimientos de descubrimiento | A; topologia inferida presentada como permanente |
| [AGENT_PACKAGE/NETWORK_TOPOLOGY_SNAPSHOT_2026-08-26.md](../AGENT_PACKAGE/NETWORK_TOPOLOGY_SNAPSHOT_2026-08-26.md) | Foto de endpoints y agrupaciones | A; falta version de entradas; cantidades internas no reconciliadas |
| [AGENT_PACKAGE/LOGIC_DEFINITION_GUIDE.md](../AGENT_PACKAGE/LOGIC_DEFINITION_GUIDE.md) | Catalogo LOGIC_CHECK/LOGIC_ACTION/LOGIC derivado del libro | A; base util, semantica no cubierta por el libro debe quedar UNKNOWN |
| [AGENT_PACKAGE/PROTOCOL_DEFINITION_GUIDE.md](../AGENT_PACKAGE/PROTOCOL_DEFINITION_GUIDE.md) | VAR/OBJSER/VARCOND/CONTAINER y ejemplos | A; contraste necesario con libro y parser de version concreta |
| [AGENT_PACKAGE/Q_LRBG_MIGRATION.md](../AGENT_PACKAGE/Q_LRBG_MIGRATION.md) | Decision/estado de migracion parcial JRU | A; distingue nombres heredados, pasos futuros y validacion limitada |
| [AGENT_PACKAGE/jru_config/FASE_1_REPORTE.md](../AGENT_PACKAGE/jru_config/FASE_1_REPORTE.md) | Alcanzabilidad y propuesta de reduccion | A; metodologia no acredita uso real ni seguridad de deduplicacion |
| [AGENT_PACKAGE/jru_config/FASE_2_REPORTE.md](../AGENT_PACKAGE/jru_config/FASE_2_REPORTE.md) | Clasificacion de variantes JRU | A; reconoce roles desconocidos, aun sin procedencia versionada |
| [AGENT_PACKAGE/jru_config/FASE_4A_REPORTE.md](../AGENT_PACKAGE/jru_config/FASE_4A_REPORTE.md) | Reposicion y envolturas VARCOND | A; conclusiones de conformidad con pruebas pendientes |
| [AGENT_PACKAGE/teaching-workspace/MISSION.md](../AGENT_PACKAGE/teaching-workspace/MISSION.md) | Objetivo docente y autoridad del paquete | M; fuente unica incompatible con resolucion por evidencia |
| [AGENT_PACKAGE/teaching-workspace/CURRICULUM.md](../AGENT_PACKAGE/teaching-workspace/CURRICULUM.md) | Ruta de aprendizaje por modulos | M; buena organizacion, depende de exactitud de referencias |
| [AGENT_PACKAGE/teaching-workspace/NOTES.md](../AGENT_PACKAGE/teaching-workspace/NOTES.md) | Notas de progresion y reglas de trabajo | M; decisiones docentes no prueban semantica del sistema |
| [AGENT_PACKAGE/teaching-workspace/RESOURCES.md](../AGENT_PACKAGE/teaching-workspace/RESOURCES.md) | Mapa de materiales | M; entrada util, sin jerarquia de evidencias |
| [AGENT_PACKAGE/teaching-workspace/learning-records/0001-arquitectura-y-serialización-encoder.md](../AGENT_PACKAGE/teaching-workspace/learning-records/0001-arquitectura-y-serializaci%C3%B3n-encoder.md) | Registro de aprendizaje del encoder | M; refuerza compresion y bytes erroneos; no es testimonio experto independiente |
| [tools/USER_GUIDE.md](../tools/USER_GUIDE.md) | Uso del lector y banco demo | A; conserva puente/orden de arranque, promete consistencia excesiva |
| [wireshark_filter_generator/README.md](../wireshark_filter_generator/README.md) | Instalacion/uso/exportacion del generador | A; ejemplos de puertos y promesas de cobertura/UI no coinciden con codigo |
| [wireshark_filter_generator/ECOSYSTEM_REFERENCE.md](../wireshark_filter_generator/ECOSYSTEM_REFERENCE.md) | Explicacion de red y VarDB para el generador | A; duplica arquitectura, compresion y gateways no demostrados |

Tambien se leyo [AGENT_PACKAGE/INDEX.txt](../AGENT_PACKAGE/INDEX.txt). Es un indice de navegacion y repite afirmaciones de las referencias; no aporta evidencia independiente.

## Configuraciones

Se parsearon los 254 XML de contenido sin errores de sintaxis. Se extrajeron todas las CNX y cantidades de acciones. Se comprobaron 72 definiciones SYNC para existencia adyacente de protocolo/logica y destinos serobj_ident contra id/ident. Esto no cubre todas las referencias, los includes, la semantica de estados ni la aceptacion por el parser del producto.

Los conteos de la siguiente tabla son archivos de cualquier extension en cada directorio del paquete. S/R son definiciones de SENDSEROBJ_LA/RECEIVESEROBJ_LA, no ejecuciones por ciclo ni total de todas las acciones posibles.

| Familia | Archivos | S/R | Conocimiento y punto de control |
|---|---:|---:|---|
| ATO | 6 | 4/2 | Varias salidas por instancia; filtro pierde una |
| BLT | 8 | 0/1 | Entradas 51611/52611/53611; no asumir enlace directo BTM |
| BTM | 13 | 6/3 | inout y multiples salidas; reduccion a par RX/TX es insuficiente |
| DI | 6 | 1/1 | Lectura completa de protocolo/logica/sync simplificados; contenedores usados ausentes |
| DMI | 8 | 8/2 | inout, protocolo por capas y buffer MAMUT; revision semantica completa pendiente |
| DO | 8 | 1/1 | inout con puertos local/remoto distintos |
| EBAL | 5 | 4/0 | Cuatro destinos; dependencia X21 no adyacente |
| ENC | 6 | 1/0 | Recorrido detallado de envio/normal/forced/layout42 |
| ER | 6 | 0/3 | EUR_CC y UDP_CC; no reducir todos los canales a UDP |
| JRU | 21 | 7/5 | Multiples variantes; SYNC paquete selecciona test2 |
| LED | 8 | 0/1 | inout declarado no implica logica bidireccional |
| MVB | 4 | 1/1 | Copias XML diferentes; requiere considerar acciones especializadas |
| RAD | 6 | 2/0 | Dos destinos por canal; uno se pierde en filtros |
| RBC | 5 | 0/1 | Endpoints cruzados con ER, no validar unicamente por numero de puerto |
| RBD | 4 | 1/1 | inout con destino compartido |
| RED | 4 | 1/1 | inout con destino compartido |
| STM | 4 | 7/7 | STM_CC; semantica de transporte no equivalente a UDP_CC |
| VEGA | 8 | 1/0 | CNX input existe, pero no hay RECEIVESEROBJ_LA en esta logica |

La carpeta plana del generador contiene 36 archivos conn_config reconocidos como instancias por su parser. El numero no valida que sus filtros incluyan todos los canales. Las copias distintas por hash pertenecen a DI, ENC, JRU y MVB (cuatro XML en cada familia); distinguir normalizacion textual de cambios funcionales queda pendiente.

## Codigo, tests y artefactos

| Superficie | Cobertura efectiva | Pendiente / limite |
|---|---|---|
| [Src/simul_handler.cpp](../Src/simul_handler.cpp), [Src/simul_executer.cpp](../Src/simul_executer.cpp) | Rutas de carga, supervision, stop, trabajador y destructor leidas | No build ni prueba de lifecycle; bibliotecas CAF externas |
| [Src/config_file_parser.cpp](../Src/config_file_parser.cpp), [Src/vdbsync_params.cpp](../Src/vdbsync_params.cpp) | Contraste focal de tokens, lectura y parametros | No auditoria completa de memoria/errores de todo el parser |
| [Make/Makefile](../Make/Makefile) y proyecto MSVC | Make leido: depende de arbol Caesar externo; proyecto inventariado | Trazabilidad build->binario UNKNOWN |
| [tools/vardb_reader.py](../tools/vardb_reader.py) | Lectura completa; prueba sintetica de inestabilidad | No lectura live, arrays, stress concurrente ni prueba integral de offsets/limites |
| [tools/start_demo.bat](../tools/start_demo.bat), [tools/stop_demo.bat](../tools/stop_demo.bat) | Lectura completa de comandos/efectos | No ejecutados; sleeps no demuestran readiness |
| Generador: parser, filter_generator, main, exporters | Lectura completa; ejecucion en memoria de parser/filtros reales | Exportadores no ejecutados; validacion Wireshark GUI pendiente |
| [wireshark_filter_generator/tests/test_parser.py](../wireshark_filter_generator/tests/test_parser.py) | Cuatro tests leidos | No ejecutados aqui; no cubren multicanal/inout ni estabilidad VarDB |
| [AGENT_PACKAGE/network_visualizer.py](../AGENT_PACKAGE/network_visualizer.py) | Lectura completa, detectada clave que colapsa instancias | No ejecutar main: sobrescribe topologia |
| [.analyze_reachability_v2.py](../.analyze_reachability_v2.py), [.fase4a2_varcond.py](../.fase4a2_varcond.py) | Lectura de metodologia y transformacion | No ejecutados: escriben archivos; otros scripts JRU pendientes |
| [TP_AppEVC_2000.mmts](../TP_AppEVC_2000.mmts), copia anidada | Estructura, pasos seleccionados, TEST completo, buffers y diff | No ejecucion MAMUT; no cobertura semantica completa de todas las aserciones |
| TP2202, variante apagado, TP7788 y TP1894 de muestra | Scan de THREAD/TEST/includes y papel en el corpus | Analisis de comportamiento y evidencias individuales pendiente |
| Comunes MAMUT del paquete | Constantes, conexiones y thread de configuracion contrastados por fragmentos | Falta resolver toda la clausura de includes y macros |
| Registro TP2000 de raiz y log anidado de octubre | Fragmentos de pasos, tiempos, checks y NID_LRBG examinados | No asociacion demostrada a hashes de configuracion/binarios |
| XLSX y derivados excel_data/excel_analysis/sheets | Libro abierto como ZIP/XML de lectura; filas clave de Logica y Protocolo contrastadas con guias | No se verifico toda la extraccion celda por celda |
| test_simple_config, vardb_probe y Python auxiliar restante | Inventariados; ejemplos aislados, sondas, logs y herramientas | Revision funcional/efectos secundarios pendiente, P04 |
| JSON/DAT/canales, PDF, EXE/DLL, filtros/Lua/CSV generados | Inventariados; rol deducido por ubicacion y consumidores conocidos | No decodificacion completa, autenticidad/build o comparacion de todas las salidas |
| Claves Euroradio | Solo existencia y nombres | Contenido excluido; requiere politica de distribucion |

## Fuentes externas y contexto

EXT-01: curso principal de OneDrive, bajo Bridge/27_09, nombre Simulaors.md. Indice y busquedas globales; lectura focal de arquitectura, serializacion y movimiento; capitulo 11 completo. Se constataron correcciones recientes y errores persistentes. No se presenta como lectura integral de sus aproximadamente 6472 lineas.

EXT-02: variantes del curso, backups y PDF del mismo directorio. Existencia/tamanos inventariados; no comparados semanticamente. Un backup no se presume obsoleto por su sufijo.

EXT-03: ANALISIS_TRAINSIMULPRF_INTEGRACION_CURSO y TestProceduresAppEVC_Common externos. Identificados; parte de su contenido aparece en el curso y el contexto historico, pero revision directa completa pendiente. No contar esa citacion indirecta como lectura del original.

EXT-04: manual CA.09.010 STLib2 Lenguaje Script y fuentes externas TrainSimulPrf/TrainSignalFbck/TrainSim/Caesar citadas por el curso y la memoria. No reconsultados integralmente en este corte. La conformidad normativa y de ABI no se declara VERIFIED por esas menciones.

CTX-01: resumen de conversacion facilitado y memorias de repositorio sobre MAMUT, exploracion/hardening y migracion del generador. Consultados como orientacion secundaria. No se examino todo el transcript ni se atribuyen sus relatos a una nueva ejecucion.

## Puertas pendientes para cerrar FASE 0

| ID | Area relevante aun abierta | Siguiente comprobacion y condicion de cierre |
|---|---|---|
| P01 | Curso completo, variantes, PDF y analisis de integracion | Leer secciones restantes, comparar versiones y registrar que copia gobierna cada afirmacion |
| P02 | Lenguaje/normativa y fuentes externas | Recuperar manual/version, contrastar READ/tiempos/FORCE y reglas JRU; identificar clausulas o dejar UNKNOWN aprobado |
| P03 | Seleccion y semantica JRU | Comparar variante seleccionada/test2 con candidata aprobada; revisar todos los scripts de migracion, consumidores y pruebas originales |
| P04 | Herramientas auxiliares y banco simple | Leer Python restante, logs/config y entradas/salidas; documentar efectos sin ejecutar acciones mutadoras |
| P05 | Tests y clausura de includes | Analizar TP2202/7788/1894 y comunes faltantes; unir cada escenario a perfil, cfg y registros |
| P06 | Corpus XML duplicado y protocolos complejos | Diff semantico de las 16 copias; resolver dependencias, includes y contratos de DMI/BTM/ER/STM/MVB mas alla del parseo |
| P07 | Evidencias y procedencia de artefactos | Revisar registros completos, fuentes de hardening, builds y capturas; asociar hashes o registrar ausencia comprobada |
| P08 | Gobierno y distribucion | Validacion humana de autoridad, expertos/revisores, secretos y limites del paquete portable |

P01-P07 no se convierten en completados por entregar este informe. P08 requiere decision humana. Las nuevas pruebas de banco no son requisito automatico para auditar documentos: si falta evidencia, se registra UNKNOWN; ejecutarlas seria una accion adicional que necesita autorizacion.