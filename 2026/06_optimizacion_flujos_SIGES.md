# Objetivo 6 — Optimización de flujos críticos frontend

## Estado honesto

Implementé tres cambios de SIGES Web que reducen espera o solicitudes innecesarias. Los commits permiten demostrar la mejora técnica; todavía debo adjuntar evidencia del tiempo percibido, la métrica de rendimiento y el uso concreto de IA. Esta ficha separa ambos tipos de evidencia.

Las capturas o videos deben hacerse con datos no sensibles. No adjuntar nombres, correos, contratos ni información de clientes visibles.

## 1. Home — carga progresiva de tarjetas

- Commit: [`fc237cffa789bb2248674a9732bec395d637f00a`](https://github.com/foxdieCR/sigesWeb/commit/fc237cffa789bb2248674a9732bec395d637f00a), 22-jul-2026.
- Cambio comprobable: antes la pantalla esperaba con `Promise.all` las consultas de contadores antes de pintar tarjetas. Implementé tarjetas disponibles primero y actualicé cada contador individualmente cuando termina su consulta; agregué un indicador de carga por contador.
- Impacto técnico esperado: la navegación deja de depender de la consulta más lenta para mostrar las opciones principales.

Evidencia por adjuntar:

- [ ] Captura o video actual de Home donde las tarjetas se muestran y un contador aún carga.
- [ ] En DevTools > Network, captura con las solicitudes de contadores y su duración.
- [ ] Nota breve: problema observado antes, resultado observado después y si IA ayudó, indicando para qué se usó.
- [ ] Validación funcional: tarjetas navegables y contador correcto al finalizar.

## 2. Companies — búsqueda remota paginada y con debounce

- Commit: [`b6ce0c6894ea5cc573547cd0c1323686c8d6d00f`](https://github.com/foxdieCR/sigesWeb/commit/b6ce0c6894ea5cc573547cd0c1323686c8d6d00f), ticket `SIGE-1224`, 22-jul-2026.
- Cambio comprobable: antes se solicitaba la lista de compañías completa y se filtraba texto/país en cliente. Implementé consulta a `cmCompanies` con `skip` y `limit` de 10, envío de `text` y `countryId` al API, y una espera de 300 ms desde la última tecla antes de buscar.
- Impacto técnico esperado: menos datos iniciales, paginación desde API y menos solicitudes mientras la persona escribe.

Evidencia por adjuntar:

- [ ] Captura de la lista con paginación activa.
- [ ] DevTools > Network: solicitud `cmCompanies` con `skip`, `limit=10` y, al buscar, `text`.
- [ ] Video o dos capturas: escribir varias letras rápido y comprobar una sola solicitud tras aproximadamente 300 ms.
- [ ] Nota breve: volumen o lentitud anterior conocida, resultado actual y uso concreto de IA, si aplica.
- [ ] Validación funcional: búsqueda, país, tipo y cambio de página conservan resultados correctos.

## 3. Contracts — búsqueda con debounce

- Commit: [`987ecb27ea0e42f1a55bbb2cafbb24577e2930e6`](https://github.com/foxdieCR/sigesWeb/commit/987ecb27ea0e42f1a55bbb2cafbb24577e2930e6), 21-jul-2026.
- Cambio comprobable: incorporé `debouncedSearchQuery` con espera de 1,000 ms; la consulta de contratos usa ese valor en vez del texto de cada pulsación.
- Impacto técnico esperado: reduce solicitudes consecutivas de búsqueda cuando se escribe.

Evidencia por adjuntar:

- [ ] DevTools > Network: video o captura temporal que muestre una búsqueda luego de aproximadamente 1 s de pausa.
- [ ] Captura de resultados filtrados.
- [ ] Nota breve: problema previo, resultado actual y uso concreto de IA, si aplica.
- [ ] Validación funcional: búsqueda, país, estado/tipo y paginación funcionan juntos.

## Registro para el Excel o Drive

Completar una fila por flujo cuando se tenga evidencia real:

| Flujo | Antes verificable | Después verificable | Evidencia adjunta | Uso de IA declarado por Luis | Estado |
| --- | --- | --- | --- | --- | --- |
| Home | `Promise.all` bloqueaba el render de tarjetas hasta resolver contadores. | Tarjetas visibles primero; contadores se actualizan individualmente. |  |  | Pendiente |
| Companies | Carga completa y filtro local de texto/país. | API paginada, búsqueda remota y espera de 300 ms. |  |  | Pendiente |
| Contracts | Búsqueda solicitaba usando el valor escrito inmediatamente. | Búsqueda enviada tras 1,000 ms de pausa. |  |  | Pendiente |

## Criterio de cierre

Marcaré el objetivo 6 como cumplido cuando cada flujo tenga: cambio técnico comprobable, una prueba visual o de Network del después, una comparación razonable del antes y después, y mi explicación real de cómo utilicé IA. Si no usé IA en un flujo, lo registraré así.
