# Comprobaciones y evidencia de este corte

Fecha: 2026-11-14. Todas las comprobaciones descritas aqui se realizaron sin arrancar simuladores. Las reproducciones Python usaron el interprete .venv de este workspace y sys.dont_write_bytecode=True antes de importar modulos locales. No se instalaron dependencias, no se abrieron mappings live ni se ejecutaron migradores/exportadores.

## V01. Inventario y XML

Enumeracion de contenido excluyendo .git, .venv, .pytest_cache, .mypy_cache, .ruff_cache, __pycache__ y llm-wiki-audit: 434 archivos; 27 Markdown y 254 XML. Conteo inicial que incluia una cache anidada fue descartado y repetido con exclusiones por segmento.

Los 254 XML se cargaron mediante System.Xml.XmlDocument con XmlResolver=null. Resultado: 254 parseables, sin errores reportados. Se extrajeron CNX y acciones de las 18 familias. Es evidencia sintactica/estructural, no prueba de carga en GenericSimulator.

Para las 72 definiciones SYNC se comprobaron archivos adyacentes y serobj_ident contra ambos atributos id e ident. La primera consulta limitada a id generaba falsos positivos y fue descartada. El resultado corregido es:

- DI del paquete: ausentes cnt_DISIM_SET_DIGINPUT_VALUES y cnt_DISIM_STATUS en su protocolo seleccionado. La lectura completa del XML DI confirma la ausencia.
- EBAL_X21 del paquete: protocol_config_EBAL_X21.xml no existe al lado del SYNC. No se comprobo una regla externa de busqueda que pudiera resolverlo.
- JRU del paquete: SYNC selecciona protocol_JRU_test2.xml; 12 usos de acciones apuntan a 11 nombres ausentes: pr_JRU_discovery, pr_JRU_discovery_unit_serial, pr_RCV_JRU_MSG_1 a pr_RCV_JRU_MSG_5, pr_JRU_status_response, pr_JRU_data_ack, pr_JRU_status_sync_request y pr_JRU_invalid.
- SYNC JRU del generador selecciona protocol_JRU.xml. No es el mismo conjunto de entrada.

El chequeo no resuelve automaticamente otras clases de referencias, registros dinamicos ni semantica del parser. No se afirma que todos los XML restantes sean funcionalmente validos.

## V02. Perdida de canales en el generador

Metodos ejecutados en memoria: parse_all_configs, parse_conn_config y generate_filter_set, sobre la carpeta SimulConfig del generador. Resultado: 36 instancias reconocidas.

| Instancia | Canales declarados | Resultado generado relevante |
|---|---|---|
| ATO1 | output 60005, output 60003, input 60501 | BOTH solo 60501 o 60003; pierde 60005 |
| RAD_ch1 | output 51221 y 51222 | Solo 51222 |
| EBAL | outputs 50601, 50602, 50603, 50604 | Solo 50604 |
| BTM_cabA_1 | inout destino50311/local50411, outputs51601/52601/53601, input50601 | BOTH solo 50601 o 53601 |
| DMI_cabA_1 | inout destino50311/local50511 | RX y TX iguales: udp.port == 50311 |

Origen: InterfacePair conserva un unico input_interface y output_interface; group_interfaces sobrescribe los anteriores. Los filtros usan udp.port y no distinguen direcciones. No se ejecuto exportacion ni se modificaron sus salidas existentes.

La suite existente de cuatro tests se leyo, no se ejecuto en esta fase; no cubre estos casos multicanal. No confundir sus resultados historicos con esta reproduccion.

## V03. Consistencia del lector

Se construyo una instancia sintetica sin llamar a __init__, sin handle ni vista compartida. Se declaro VALUE como escalar tipo6 (float) y se sustituyo _bytes por un iterador de cuatro bloques validos distintos con valores 1, 2, 3, 4 y calidad2.

Resultado de read('VALUE'): VarValue(value=4.0, quality='OK', forced=False). Ningun par de bloques consecutivos coincidio. Por tanto, agotar los tres reintentos no produce error de inestabilidad: se devuelve el ultimo bloque. quality='OK' procede del byte de calidad, no de haber obtenido una lectura atomica.

Esto falsifica la garantia documental de consistencia absoluta. No demuestra una lectura rota real en el banco, ni mide frecuencia, rendimiento o interferencia. Tampoco demuestra que dos bloques iguales basten para atomicidad frente a cualquier escritor.

## V04. Conversion numerica

Comprobado con struct de Python:

| Operacion | Resultado |
|---|---|
| pack('>f', 150.5) | 43 16 80 00 |
| unpack('<f', bytes 00 80 16 43) | 150.5 |
| unpack('<I', bytes 05 0e 00 00) | 3589, no 5 |

El payload ENC se calcula separadamente desde los objetos seleccionados: cabecera 16+32+16 bits, numero de encoder16, cuatro velocidades32 y cuatro estados32 = 336 bits = 42 bytes. Primer campo velocidad en offset10 del payload. No se infiere el tamano del mensaje a partir de todas las VAR declaradas ni del tamano de la VarDB.

La observacion previa de UDP length50/payload42 y periodo aproximado31ms procede del contexto historico y de la guia; no hubo una captura nueva en esta fase. Offset0x2A de una trama solo aplica a la encapsulacion concreta descrita, no a todas las capturas.

## V05. Test y registro TP2000

Scan y lectura del TEST principal: 36 THREAD de paso, dos TEST y una TEST_SUITE. Grupos paralelos 8-12, 15-16, 24-25 y 32-34. La espera de autotest compara 60 con ELAPSED_TIME menos Timer_1. Paso9 termina antes del START del13. Las comprobaciones DMI seleccionadas usan OR para aceptar al menos un buffer.

Diff no-index entre copias: la anidada anade cabecera y tres identificadores JRU al campo REQ; no cambia la secuencia ejecutable de pasos. Conteo de requisitos concatenados: raiz24, anidada27.

Fragmento observado de [TP_AppEVC_2000_mammut_out.txt](../TP_AppEVC_2000_mammut_out.txt): paso32 inicia73.093557, paso34 termina75.493865, paso35 muestra distancia1.041898 y paso36 distancia1.070639; ultimo END TIME77.553880. Son datos de ese registro, no tiempo garantizado de todos los ensayos. No se certifica resultado global del test ni correspondencia con el XML actual solo a partir de ese fragmento.

En el log anidado de octubre se observa un diagnostico sobre GENERAL_MESSAGE_NID_LRBG y valor16777215. El texto del log no demuestra por si solo la causa ni la correccion de una migracion posterior.

## V06. Libro y metodologia JRU

Libro XLSX original abierto mediante ZipFile.OpenRead y XML de hojas/sharedStrings, sin Excel ni escritura. Filas contrastadas: Logica2-4,7,14,18; Protocolo2,5,6,8,10. La hoja Protocolo define size en bits y COND mediante idserobj/operator=equal/value. La hoja Logica menciona get_last_msg y muestra ejemplos, pero no ofrece una especificacion completa de sus valores.

Lectura de .fase4a2_varcond.py: emite COND var_id/op=EQ y anida VAR bajo VARCOND. No coincide con el contrato descrito por el libro. No se ejecuto el script ni el parser real; su compatibilidad runtime queda UNKNOWN.

Lectura de .analyze_reachability_v2.py: recorre descendientes desde INTERFACE y usa un mapa de elementos con id; muchas referencias OBJSER/COND sin id quedan fuera del recorrido de referencias. Una sola pasada de referencias tampoco calcula clausura transitiva general. Su resultado100% no acredita que todos los elementos sean consumidos por mensajes/acciones.

## V07. Identidad de copias

SHA-256 observados:

| Fuente | SHA-256 |
|---|---|
| Referencia raiz | F49F95F7C10AD6A79FE5CB51D5EDE904CDFA74B48DBD169936171C06D200E49D |
| Referencia del paquete | 9BA73B57B38BE5541DC14F2573AA0FA141E228206E6DA1417896B1F96927BE1F |
| TP2000 raiz | 8BB2B82B9C52AA3F27F132A1328A2DD20489C48944A8B82A037CCBEB79704919 |
| TP2000 anidado | E697F28E78750D160B8728714C0BF342452AA92B4773CC9052192D6D36B7BF9B |

Comparacion de XML homonimos paquete/generador: difieren conn, logic, protocol y sync de DI, ENC, JRU y MVB. El hash establece identidad de bytes, no importancia semantica ni autoridad.

## Restricciones y validacion del informe

Las unicas ediciones de esta auditoria se limitan a informes dentro de llm-wiki-audit. No se modifican originales para resolver los defectos. No se eliminan, renombran o reorganizan archivos; no hay commit, nueva rama, RAG, base de datos o grafo. No se ejecutaron start_demo, stop_demo, migradores JRU ni main de generadores que escriben salidas.

Validacion realizada: 13 secciones en orden, enlaces locales existentes y ningun diagnostico del editor en los tres informes. El recuento final conserva 434 archivos fuente. La comprobacion detecto y permitio corregir el subtotal de otros archivos a 37. git diff --name-only no mostro cambios en archivos seguidos; git status conserva las entradas no seguidas previas y anade este directorio de auditoria.

El estado Git no constituye por si solo una prueba de integridad de todos los archivos no seguidos. No se presenta como una comparacion criptografica completa antes/despues. La restriccion de solo informes se ha respetado en las acciones realizadas; la equivalencia de todos los artefactos externos no se certifica.