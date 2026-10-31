# orma-keepwarm

Mantiene despiertos los servicios Free de Render de ORMA staging sin pagar plan.

- CMS: https://orma-cms-staging.onrender.com/_health

Render duerme cada servicio tras 15 min sin trafico entrante y tarda ~80 s en volver; por eso la
primera visita del dia costaba mas de un minuto. Este workflow pingea cada 5 min solo dentro de la
ventana 8:00-18:00 de CDMX (= 14:00-23:50 UTC). El gateway de lectura se retiro y el sitio habla
directo con el CMS, asi que el CMS es el unico servicio que hay que mantener despierto: 10 h/dia son
~300 h/mes de las 750 h del plan Free, lo que deja margen para los demas servicios del workspace.

Cada run se mantiene vivo (maximo 6 h por job) y hace el ping en bucle desde dentro, asi evitamos
depender de la puntualidad de los schedules de GitHub. Consumo: ~580 min/mes de runner privado,
dentro de los 2,000 min/mes gratuitos.

Prueba manual: Actions -> keep-warm -> Run workflow (minutes = 0 hace un solo ping).

Nota: GitHub desactiva los workflows programados tras 60 dias sin actividad en el repositorio.
Si deja de correr, basta un commit trivial o `gh workflow enable keep-warm`.
