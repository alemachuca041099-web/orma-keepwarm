# orma-keepwarm

Mantiene despiertos los servicios Free de Render de ORMA staging sin pagar plan.

- CMS: https://orma-cms-staging.onrender.com/_health

Render duerme cada servicio tras 15 min sin trafico entrante y tarda ~80 s en volver; por eso la
primera visita del dia costaba mas de un minuto. Este workflow pingea cada 5 min dentro de la
ventana 5:00-23:00 de CDMX (= 11:00-05:00 UTC). El gateway de lectura se retiro y el sitio habla
directo con el CMS, asi que el CMS es el unico servicio que hay que mantener despierto.

Se deja dormir de 23:00 a 5:00 a proposito: 24/7 serian 744 h de las 750 h del plan Free, sin
margen para el otro servicio web del workspace, y agotar la cuota hace que Render suspenda todos
los servicios hasta el siguiente ciclo. 18 h/dia son ~547 h/mes y dejan ~200 h de colchon.

Cada run se mantiene vivo (maximo 6 h por job) y hace el ping en bucle desde dentro, asi evitamos
depender de la puntualidad de los schedules de GitHub. Son tres jobs de 5h55 al dia; los huecos de
5 min entre ellos quedan muy por debajo del timer de 15 min de Render. El repositorio es publico a
proposito: Actions es ilimitado en repos publicos y esta ventana necesita ~32,000 min/mes, muy por
encima de los 2,000 min/mes gratuitos de un repo privado.

Prueba manual: Actions -> keep-warm -> Run workflow (minutes = 0 hace un solo ping).

Nota: GitHub desactiva los workflows programados tras 60 dias sin actividad en el repositorio.
Si deja de correr, basta un commit trivial o `gh workflow enable keep-warm`.
