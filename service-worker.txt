const CACHE_NAME = 'wochentage-spiel-github-v16';
const APP_SHELL = ['./','./index.html','./manifest.json','./icon-192.png','./icon-512.png'];

function patchAbenteuer(html) {
  return html
    .replace(
      'const track=[[16.5,24,"move"],[26,19.5,"Montag"],[36.5,20,"card"],[43,22,"football"],[54,22.5,"Dienstag"],[64,22,"move"],[70,22,"star"],[80.5,23.5,"Mittwoch"],[88,31.5,"card"],[91,40.5,"football"],[92,51,"Donnerstag"],[92,62.5,"move"],[90,70,"star"],[80,77,"Freitag"],[67,82.5,"football"],[59.5,83,"card"],[46,82,"Samstag"],[33.5,78,"move"],[28,72,"star"],[17.5,70,"Sonntag"],[8,64,"card"],[7,56.5,"move"]];',
      'const track=[[16.1,24.2,"move"],[25.9,17.6,"Montag"],[36.2,19.6,"card"],[43.3,21.2,"football"],[54.3,22.5,"Dienstag"],[63.8,22.6,"move"],[70.0,22.6,"star"],[79.7,23.6,"Mittwoch"],[87.7,32.5,"card"],[91.2,40.0,"football"],[92.5,51.7,"Donnerstag"],[92.6,62.9,"move"],[90.5,69.9,"star"],[80.5,77.4,"Freitag"],[67.7,81.8,"football"],[59.2,83.7,"card"],[47.1,82.3,"Samstag"],[34.4,79.0,"move"],[28.4,73.4,"star"],[17.9,70.5,"Sonntag"],[8.3,64.3,"card"],[7.0,57.0,"move"]];'
    )
    .replace('const startCenter=[7.5,27.5];','const startCenter=[7.5,29.5];')
    .replace('const goalCenter=[15.5,49.5];','const goalCenter=[16.3,49.2];')
    .replace('const goalSlots=[[7.5,49.5],[25,50.5]];','const goalSlots=[[7.9,48.7],[24.7,49.7]];');
}

self.addEventListener('install', event => {
  event.waitUntil(caches.open(CACHE_NAME).then(c => c.addAll(APP_SHELL)).then(() => self.skipWaiting()));
});

self.addEventListener('activate', event => {
  event.waitUntil(caches.keys().then(keys => Promise.all(keys.filter(k => k !== CACHE_NAME).map(k => caches.delete(k)))).then(() => self.clients.claim()));
});

self.addEventListener('fetch', event => {
  if (event.request.method !== 'GET') return;

  const url = new URL(event.request.url);
  const isAbenteuer = url.pathname.endsWith('/abenteuer.html');

  if (isAbenteuer) {
    event.respondWith(
      fetch(event.request)
        .then(async r => {
          const html = await r.text();
          const patched = patchAbenteuer(html);
          const response = new Response(patched, {
            status: r.status,
            statusText: r.statusText,
            headers: {'Content-Type':'text/html; charset=utf-8'}
          });
          const copy = response.clone();
          caches.open(CACHE_NAME).then(c => c.put(event.request, copy));
          return response;
        })
        .catch(() => caches.match(event.request))
    );
    return;
  }

  if (event.request.mode === 'navigate') {
    event.respondWith(fetch(event.request).then(r => {
      const copy=r.clone(); caches.open(CACHE_NAME).then(c=>c.put('./index.html',copy)); return r;
    }).catch(() => caches.match('./index.html')));
    return;
  }

  event.respondWith(caches.match(event.request).then(cached => cached || fetch(event.request).then(r => {
    if (r && r.status===200) { const copy=r.clone(); caches.open(CACHE_NAME).then(c=>c.put(event.request,copy)); }
    return r;
  })));
});
