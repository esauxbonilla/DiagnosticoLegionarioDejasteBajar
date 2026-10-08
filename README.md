# ¿Por qué dejaste de bajar? — Diagnóstico · Bastián Almaraz

Lead magnet de una sola página (`index.html`, sin dependencias). Se publica tal cual en GitHub Pages, Netlify, Vercel o cualquier hosting estático.

## Flujo
Portada → 10 preguntas → edad y sexo → usuario de Instagram → resultado.
El CTA de Instagram (“YO”) solo aparece si encaja con el perfil ideal: hombre, 27–45 años y que ya haya intentado algo o lleve 2+ semanas estancado.

## Guardar los leads (usuario de IG + respuestas)
Sin esto, el usuario de Instagram **no se guarda en ningún lado**.

1. Crea una Google Sheet → Extensiones → Apps Script, pega esto y guarda:

```js
function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  const sh = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
  const keys = Object.keys(d).filter(k => k !== 'puntajes');
  if (sh.getLastRow() === 0) sh.appendRow(keys.concat(['puntajes']));
  sh.appendRow(keys.map(k => d[k]).concat([JSON.stringify(d.puntajes)]));
  return ContentService.createTextOutput('ok');
}
```

2. Implementar → Nueva implementación → Aplicación web → Ejecutar como: tú · Acceso: Cualquier usuario → copia la URL.
3. En `index.html`, pega la URL en `CONFIG.WEBHOOK_URL`.

También funciona con Make, Zapier o n8n (webhook que reciba JSON por POST). Los parámetros `utm_source`, `utm_medium` y `utm_campaign` de la URL se guardan junto con cada lead.
