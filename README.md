<!DOCTYPE html>
<html lang="vi"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Craft Mine</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&subset=vietnamese&display=swap">
<style>
:root{--ui:rgba(30,30,30,.7);box-sizing:border-box}
*{border-radius:0}
html,body{height:100%;margin:0;overflow:hidden;background:#87ceeb;font-family:system-ui,sans-serif;touch-action:none;user-select:none;-webkit-user-select:none}
canvas#g{display:block;width:100%;height:100%}
button{font-weight:bold;color:#fff;background:#7a7a7a;border:3px solid;border-color:#c8c8c8 #3a3a3a #3a3a3a #c8c8c8;touch-action:none}
button:active,button.on{background:#4f6fa0}
#info{position:fixed;top:calc(6px + env(safe-area-inset-top,0px));left:8px;right:64px;color:#fff;background:var(--ui);padding:4px 8px;font-size:11px;pointer-events:none}
#hp{position:fixed;top:calc(32px + env(safe-area-inset-top,0px));left:8px;font-size:12px;pointer-events:none;text-shadow:0 0 3px #000;color:#fff;line-height:1.3}
#hud{position:fixed;left:0;right:0;top:calc(76px + env(safe-area-inset-top,0px));display:flex;justify-content:center;gap:3px}
.s{width:42px;height:42px;border:3px solid #555;background:var(--ui);position:relative;box-sizing:border-box}
.s.on{border-color:#fff}.s img{width:100%;height:100%;image-rendering:pixelated;display:block}
.s i{position:absolute;right:2px;bottom:0;color:#fff;font:bold 12px sans-serif;text-shadow:1px 1px #000}
#cross{position:fixed;left:50%;top:50%;width:16px;height:16px;margin:-8px;pointer-events:none}
#cross:before,#cross:after{content:"";position:absolute;background:#fff;mix-blend-mode:difference}
#cross:before{left:7px;top:0;width:2px;height:16px}#cross:after{top:7px;left:0;height:2px;width:16px}
#ring{position:fixed;left:50%;top:50%;width:48px;height:48px;margin:-24px;border-radius:50%;display:none;pointer-events:none;background:conic-gradient(#fff calc(var(--p,0)*360deg),rgba(255,255,255,.25) 0);-webkit-mask:radial-gradient(circle,transparent 17px,#000 18px);mask:radial-gradient(circle,transparent 17px,#000 18px)}
#pad{display:none;position:fixed;left:10px;bottom:calc(14px + env(safe-area-inset-bottom,0px));grid-template-columns:repeat(3,52px);gap:4px}
#pad button{height:52px;font-size:20px}
#jump{display:none;position:fixed;right:16px;bottom:calc(24px + env(safe-area-inset-bottom,0px));width:76px;height:76px;font-size:26px}
#dots{position:fixed;top:calc(6px + env(safe-area-inset-top,0px));right:8px;width:48px;height:40px;font-size:22px}
#inv{display:none;position:fixed;inset:0;background:rgba(0,0,0,.82);color:#fff;overflow:auto;touch-action:pan-y;padding:calc(10px + env(safe-area-inset-top,0px)) 10px 24px}
#inv .box{max-width:440px;margin:0 auto}#inv h3{display:flex;justify-content:space-between;align-items:center;margin:10px 0 6px}
.row{display:grid;grid-template-columns:1fr 1fr;gap:6px}.row button{padding:10px 4px;font-size:13px}
.grid{display:grid;grid-template-columns:repeat(7,1fr);gap:3px}.grid .s{width:100%;height:auto;aspect-ratio:1}.grid .s.hb{background:rgba(80,120,200,.5)}
.rc{display:flex;justify-content:space-between;align-items:center;gap:8px;background:var(--ui);padding:8px;margin-bottom:6px;font-size:14px}
button.x{padding:8px 12px}button:disabled{background:#444;color:#888}
#toast{position:fixed;left:50%;top:40%;transform:translateX(-50%);background:var(--ui);color:#fff;padding:8px 14px;display:none;pointer-events:none}
</style><style>
html,body{font-family:VT323,"Courier New",monospace}
button{font:20px VT323,"Courier New",monospace;color:#e0e0e0;text-shadow:2px 2px #3f3f3f;background:#707070;border:2px solid #000;box-shadow:inset 2px 2px 0 #a8a8a8,inset -2px -2px 0 #3c3c3c;touch-action:none}
button:hover,button:active,button.on{background:#7a86c0;color:#ffffa0;box-shadow:inset 2px 2px 0 #b9c2ee,inset -2px -2px 0 #4a5390}
button:disabled{background:#4a4a4a;color:#8a8a8a;box-shadow:inset 2px 2px 0 #666,inset -2px -2px 0 #333;text-shadow:none}
#info{font-size:15px}#hp{font-size:17px}#toast{font-size:22px}.s i{font:18px VT323,monospace}
.s{border:2px solid #373737;background:rgba(0,0,0,.5)}.s.on{border:3px solid #fff}
#inv{background:rgba(0,0,0,.65)}
#inv .box{background:#c6c6c6;border:4px solid;border-color:#fff #555 #555 #fff;outline:2px solid #000;padding:12px;color:#404040}
#inv h3{color:#404040;font-size:24px;font-weight:normal}#inv p{color:#555;font-size:16px}#inv input{font:20px VT323,monospace;background:#000;color:#fff;border:2px solid #a0a0a0}
.grid .s{background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737}.grid .s.hb{background:#7d93bd}
.grid .s.on{border:2px solid #fff;box-shadow:0 0 0 3px #000}
.rc{background:#8b8b8b;border:2px solid;border-color:#373737 #fff #fff #373737;color:#fff;text-shadow:2px 2px #3f3f3f;font-size:19px}
.row button{font-size:20px}
body.title #hud,body.title #hp,body.title #pad,body.title #jump,body.title #dots,body.title #cross,body.title #info,body.title #ring{display:none!important}
#title{position:fixed;inset:0;display:none;overflow:auto;touch-action:pan-y;color:#fff;background:radial-gradient(transparent 35%,rgba(0,0,0,.6))}
body.title #title{display:flex}
.tin{margin:auto;display:flex;flex-direction:column;align-items:center;width:100%;padding:calc(18px + env(safe-area-inset-top,0px)) 0 calc(18px + env(safe-area-inset-bottom,0px))}
.logo{font:clamp(22px,8.5vw,40px) "Press Start 2P","Courier New",monospace;color:#c6c6c6;text-shadow:3px 3px 0 #3b3b3b,6px 6px 0 #161616;letter-spacing:2px;text-align:center}
.logo small{display:block;font-size:12px;color:#ffd24d;text-shadow:2px 2px 0 #3a2a00;margin-top:10px;letter-spacing:3px}
.splash{color:#ff0;font-size:24px;text-shadow:2px 2px #3f3f00;margin:2px 0 22px;animation:pl 1s infinite alternate}
@keyframes pl{from{transform:rotate(-6deg) scale(.92)}to{transform:rotate(-6deg) scale(1.05)}}
.col{display:flex;flex-direction:column;gap:10px;width:min(340px,86%)}
.mcb{display:block;width:100%;padding:9px 6px;font-size:24px;cursor:pointer;touch-action:manipulation;box-sizing:border-box}
.ttl{font:normal 30px VT323;text-shadow:2px 2px #3f3f3f;margin:0 0 10px}
.wl{width:min(420px,94%);max-height:44vh;overflow:auto;background:rgba(0,0,0,.5);border:2px solid #000;padding:6px;box-sizing:border-box}
.wr{display:flex;gap:10px;align-items:center;padding:8px;border:2px solid transparent;cursor:pointer}.wr.on{border-color:#fff;background:rgba(255,255,255,.12)}
.wr img{width:48px;height:48px;image-rendering:pixelated}.wr b{font-weight:normal;font-size:24px}.wr span{color:#aaa;font-size:17px}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:8px;width:min(420px,94%);margin-top:8px}
.form{width:min(420px,92%);display:flex;flex-direction:column;gap:6px}.form label{color:#aaa;font-size:19px}
.form input[type=text],.form input[type=number]{background:#000;color:#fff;border:2px solid #a0a0a0;font:22px VT323,monospace;padding:6px;width:100%;box-sizing:border-box}
.form input[type=range]{width:100%}
.txt{width:min(420px,92%);font-size:20px;line-height:1.25;background:rgba(0,0,0,.5);border:2px solid #000;padding:10px;box-sizing:border-box;margin-bottom:8px}
.foot{color:#bbb;font-size:15px;margin-top:20px;text-align:center;padding:0 10px}
#loading{position:fixed;inset:0;display:none;flex-direction:column;align-items:center;justify-content:center;gap:14px;color:#fff;font-size:28px;text-shadow:2px 2px #3f3f3f;background:linear-gradient(rgba(0,0,0,.55),rgba(0,0,0,.55)),var(--dirt) 0 0/96px 96px;image-rendering:pixelated;z-index:5}
#loading i{display:block;width:200px;height:8px;background:#000;border:2px solid #555;position:relative;overflow:hidden}#loading i:after{content:"";position:absolute;inset:0;width:40%;background:#6c6;animation:ld 1.2s linear infinite}
@keyframes ld{from{left:-40%}to{left:100%}}
</style>
<style>
#xpb{position:fixed;left:50%;transform:translateX(-50%);top:calc(126px + env(safe-area-inset-top,0px));width:min(300px,80vw);height:8px;background:#000;border:2px solid #373737;pointer-events:none}
#xpf{position:absolute;left:0;top:0;bottom:0;background:#7f3;display:block;width:0}
#xpl{position:absolute;left:50%;top:-20px;transform:translateX(-50%);color:#7f3;font:22px VT323,monospace;text-shadow:1px 1px #000}
#itn{position:fixed;left:0;right:0;top:calc(152px + env(safe-area-inset-top,0px));text-align:center;color:#fff;font-size:24px;text-shadow:2px 2px #3f3f3f;opacity:0;transition:opacity .5s;pointer-events:none}
#adv{position:fixed;top:calc(8px + env(safe-area-inset-top,0px));left:50%;transform:translate(-50%,-160%);transition:transform .4s;background:#222;border:2px solid #fff;padding:6px 16px;color:#fff;font-size:20px;z-index:4;pointer-events:none;text-align:center}
#adv.on{transform:translate(-50%,0)}#adv b{display:block;color:#ff0;font-weight:normal;font-size:22px}
#dbg{position:fixed;left:6px;top:calc(170px + env(safe-area-inset-top,0px));color:#fff;background:rgba(0,0,0,.5);font:15px monospace;padding:4px 6px;white-space:pre;display:none;pointer-events:none}
#flash{position:fixed;inset:0;background:rgba(200,0,0,.4);opacity:0;pointer-events:none}
body.title #xpb,body.title #itn,body.title #dbg,body.title #flash{display:none!important}
</style>
</head><body>
<canvas id="g"></canvas><div id="cross"></div><div id="ring"></div>
<div id="info">Chạm: đặt/ăn/đánh | Giữ: phá | ... : menu | Nhảy 2 lần (Sáng tạo): bay | ^ 2 lần: chạy | PC: WASD, Space, Shift, E, V</div>
<div id="hp"></div><div id="hud"></div><button id="dots">...</button>
<div id="pad"><span></span><button data-k="w">^</button><span></span><button data-k="a">&lt;</button><button id="sit">↓</button><button data-k="d">&gt;</button><span></span><button data-k="s">v</button><span></span></div>
<button id="jump">↑</button><div id="inv"></div><div id="toast"></div><div id="title"></div><div id="loading"><span id="ldt"></span><i></i></div><div id="xpb"><s id="xpf"></s><b id="xpl">0</b></div><div id="itn"></div><div id="adv"></div><div id="dbg"></div><div id="flash"></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
const cleanNick=s=>String(s||'').replace(/[^\p{L}\p{N} _-]/gu,'').trim().slice(0,14);
const LS={get:k=>{try{return localStorage.getItem(k)}catch(_){return null}},set:(k,v)=>{try{localStorage.setItem(k,v);return 1}catch(_){return 0}},del:k=>{try{localStorage.removeItem(k)}catch(_){}}};
let WL=[];try{WL=JSON.parse(LS.get('mc64_worlds'))||[]}catch(_){}
if(!WL.length&&LS.get('mc64_save_v3')){WL=[{id:'w0',name:'Thế giới 1',seed:0,mode:'survival',t:Date.now()}];LS.set('mc64_w_w0',LS.get('mc64_save_v3'));LS.set('mc64_worlds',JSON.stringify(WL))}
const GO=LS.get('mc64_go');LS.del('mc64_go');
const WORLD=GO?WL.find(w=>w.id==GO):null,playing=!!WORLD,SEED=WORLD?WORLD.seed:777,SX=((SEED*7919)%100003+100003)%100003;
let OPT={rd:4,fov:75,sens:100,snd:1};try{OPT=Object.assign(OPT,JSON.parse(LS.get('mc64_opt'))||{})}catch(_){}

// ===== Ngôn ngữ =====
const LANGS=[['vi','Tiếng Việt'],['en','English'],['es','Español'],['fr','Français'],['de','Deutsch'],['pt','Português'],['ru','Русский'],['ja','日本語'],['zh','中文'],['ko','한국어']],LN={};LANGS.forEach(l=>LN[l[0]]=l[1]);
const D={
vi:{pl:'Chơi một mình',mp:'Nhiều người chơi',op:'Tùy chọn',lg:'Ngôn ngữ',bk:'Quay lại',dn:'Xong',ws:'Chọn thế giới',nw:'Chưa có thế giới nào. Hãy tạo một thế giới mới!',ps:'Vào thế giới đã chọn',cn:'Tạo thế giới mới',de:'Xóa',dc:'Bấm lần nữa để xóa!',sv:'Sinh tồn',cr:'Sáng tạo',gm:'Chế độ: {0}',wn:'Tên thế giới',sd:'Hạt giống - Seed (để trống = ngẫu nhiên)',ca:'Hủy',mt:'Tạo thế giới có CÙNG Seed rồi mở trang này cùng lúc thì sẽ thấy nhau.',rd:'Tầm nhìn: {0} chunk',fv:'Góc nhìn FOV: {0}',sn:'Độ nhạy xoay: {0}%',so:'Âm thanh: {0}',on:'BẬT',of:'TẮT',ft:'Bản tự làm, không liên kết với Mojang hay Microsoft.',ld:'Đang tạo địa hình...',inf:'Chạm: đặt/ăn/đánh | Giữ: phá | ...: menu | Nhảy 2 lần (Sáng tạo): bay | ^ 2 lần: chạy | PC: WASD, Space, Shift, E, V, F3',mn:'Menu',cl:'Đóng',nm:'Tên:',bm:'Khối: {0} | Sinh vật gần: {1}',po:'{0} người đang chơi',al:'Chỉ có bạn (mở bản đã chia sẻ để chơi chung)',ch:'Nhân vật: {0}',vw:'Góc nhìn: {0}',v1:'Ngôi thứ 1',v3:'Ngôi thứ 3',sw:'Lưu thế giới',ex:'Lưu và thoát ra màn hình chính',hb:'Thanh nhanh (chạm ô, rồi chọn khối bên dưới)',iv:'Kho đồ',ai:'Tất cả khối & vật phẩm',ih:'Hàng xanh = thanh nhanh. Chạm ô xanh để chọn, chạm ô dưới để hoán đổi. Cầm đồ ăn rồi chạm để ăn.',cf:'Chế tác',cb:'Chế',hc:'SÁNG TẠO',fl:'đang bay',he:'Máu',hu:'Đói',sa:'Đã lưu thế giới',sf:'Không lưu được (trình duyệt chặn)',di:'Bạn đã chết! Hồi sinh ở điểm xuất phát',np:'Cần cúp tốt hơn để lấy khối này!',bo:'BÙM!',ns:'Trình duyệt chặn lưu trữ nên không tạo được thế giới',nwn:'Thế giới mới',
 adv:'Đã đạt thành tựu!',a_wood:'Lấy gỗ',a_bench:'Chế bàn thủ công',a_pick:'Đến giờ đào!',a_stone:'Thời đồ đá',a_iron:'Có sắt rồi',a_diamond:'KIM CƯƠNG!',a_kill:'Thợ săn quái vật',dbg:'Màn hình gỡ lỗi (F3): {0}',cok:'Xong',cu:'Lệnh không tồn tại. Thử /help',chelp:'Lệnh: /gamemode /time /weather /tp /give /summon /kill /seed /clear /xp /heal',
 spl:['Tự làm 100%!','Hơn 160 khối!','Cẩn thận creeper!','Giữ để phá, kéo vẫn phá!','Chạy trên điện thoại!','14 sinh vật!','Có mưa và mây!']},
en:{pl:'Singleplayer',mp:'Multiplayer',op:'Options',lg:'Language',bk:'Back',dn:'Done',ws:'Select World',nw:'No worlds yet. Create a new one!',ps:'Play Selected World',cn:'Create New World',de:'Delete',dc:'Press again to delete!',sv:'Survival',cr:'Creative',gm:'Game Mode: {0}',wn:'World name',sd:'Seed (empty = random)',ca:'Cancel',mt:'Make worlds with the SAME seed and open this page at the same time to see each other.',rd:'Render distance: {0}',fv:'FOV: {0}',sn:'Look sensitivity: {0}%',so:'Sound: {0}',on:'ON',of:'OFF',ft:'Fan-made game, not affiliated with Mojang or Microsoft.',ld:'Generating terrain...',inf:'Tap: place/eat/hit | Hold: break | ...: menu | Double-jump (Creative): fly | ^ twice: sprint | PC: WASD, Space, Shift, E, V, F3',mn:'Menu',cl:'Close',nm:'Name:',bm:'Blocks: {0} | Mobs nearby: {1}',po:'{0} players online',al:'Solo (share the page to play together)',ch:'Character: {0}',vw:'View: {0}',v1:'1st person',v3:'3rd person',sw:'Save world',ex:'Save and quit to title',hb:'Hotbar (tap a slot, then pick a block)',iv:'Inventory',ai:'All blocks & items',ih:'Blue row = hotbar. Tap to select, tap below to swap. Hold food and tap to eat.',cf:'Crafting',cb:'Craft',hc:'CREATIVE',fl:'flying',he:'Health',hu:'Hunger',sa:'World saved',sf:'Could not save (storage blocked)',di:'You died! Respawned at spawn',np:'You need a better pickaxe!',bo:'BOOM!',ns:'Storage is blocked, cannot create world.',nwn:'New World',
 adv:'Advancement Made!',a_wood:'Getting Wood',a_bench:'Benchmarking',a_pick:'Time to Mine!',a_stone:'Stone Age',a_iron:'Acquire Hardware',a_diamond:'DIAMONDS!',a_kill:'Monster Hunter',dbg:'Debug screen (F3): {0}',cok:'Done',cu:'Unknown command. Try /help',chelp:'Commands: /gamemode /time /weather /tp /give /summon /kill /seed /clear /xp /heal',
 spl:['100% homemade!','160+ blocks!','Watch out for creepers!','Hold to break, drag to keep breaking!','Runs on your phone!','14 mobs!','Now with rain and clouds!']},
es:{pl:'Un jugador',mp:'Multijugador',op:'Opciones',lg:'Idioma',bk:'Atrás',dn:'Hecho',ws:'Seleccionar mundo',nw:'Aún no hay mundos. ¡Crea uno nuevo!',ps:'Jugar mundo seleccionado',cn:'Crear mundo nuevo',de:'Eliminar',dc:'¡Pulsa otra vez para eliminar!',sv:'Supervivencia',cr:'Creativo',gm:'Modo de juego: {0}',wn:'Nombre del mundo',sd:'Semilla (vacío = aleatoria)',ca:'Cancelar',mt:'Crea mundos con la MISMA semilla y abre esta página a la vez para veros.',rd:'Distancia de renderizado: {0}',fv:'Campo de visión: {0}',sn:'Sensibilidad: {0}%',so:'Sonido: {0}',on:'SÍ',of:'NO',ft:'Juego de fans, sin relación con Mojang ni Microsoft.',ld:'Generando terreno...',inf:'Toca: poner/comer/golpear | Mantén: romper | ...: menú',mn:'Menú',cl:'Cerrar',nm:'Nombre:',bm:'Bloques: {0} | Mobs cerca: {1}',po:'{0} jugadores en línea',al:'Solo (comparte la página para jugar juntos)',ch:'Personaje: {0}',vw:'Vista: {0}',v1:'1.ª persona',v3:'3.ª persona',sw:'Guardar mundo',ex:'Guardar y salir al título',hb:'Barra rápida (toca una casilla y elige un bloque)',iv:'Inventario',ai:'Todos los bloques y objetos',ih:'Fila azul = barra rápida. Toca para elegir, toca abajo para cambiar. Con comida en mano, toca para comer.',cf:'Fabricación',cb:'Fabricar',hc:'CREATIVO',fl:'volando',he:'Salud',hu:'Hambre',sa:'Mundo guardado',sf:'No se pudo guardar',di:'¡Has muerto! Reapareces en el inicio',np:'¡Necesitas un pico mejor!',bo:'¡BUM!',ns:'El almacenamiento está bloqueado.',nwn:'Mundo nuevo'},
fr:{pl:'Un joueur',mp:'Multijoueur',op:'Options',lg:'Langue',bk:'Retour',dn:'Terminé',ws:'Choisir un monde',nw:'Aucun monde. Créez-en un !',ps:'Jouer au monde choisi',cn:'Créer un nouveau monde',de:'Supprimer',dc:'Appuyez encore pour supprimer !',sv:'Survie',cr:'Créatif',gm:'Mode de jeu : {0}',wn:'Nom du monde',sd:'Graine (vide = aléatoire)',ca:'Annuler',mt:'Créez des mondes avec la MÊME graine et ouvrez cette page en même temps pour vous voir.',rd:'Distance de rendu : {0}',fv:'Champ de vision : {0}',sn:'Sensibilité : {0}%',so:'Son : {0}',on:'OUI',of:'NON',ft:'Jeu de fans, sans lien avec Mojang ni Microsoft.',ld:'Génération du terrain...',inf:'Toucher : poser/manger/frapper | Maintenir : casser | ... : menu',mn:'Menu',cl:'Fermer',nm:'Nom :',bm:'Blocs : {0} | Créatures proches : {1}',po:'{0} joueurs en ligne',al:'Solo (partagez la page pour jouer ensemble)',ch:'Personnage : {0}',vw:'Vue : {0}',v1:'1re personne',v3:'3e personne',sw:'Sauvegarder le monde',ex:'Sauvegarder et quitter',hb:'Barre rapide (touchez une case, puis un bloc)',iv:'Inventaire',ai:'Tous les blocs et objets',ih:'Ligne bleue = barre rapide. Touchez pour choisir, touchez en dessous pour échanger. Nourriture en main : touchez pour manger.',cf:'Fabrication',cb:'Fabriquer',hc:'CRÉATIF',fl:'vol',he:'Vie',hu:'Faim',sa:'Monde sauvegardé',sf:'Sauvegarde impossible',di:'Vous êtes mort ! Retour au départ',np:'Il vous faut une meilleure pioche !',bo:'BOUM !',ns:'Stockage bloqué, impossible de créer.',nwn:'Nouveau monde'},
de:{pl:'Einzelspieler',mp:'Mehrspieler',op:'Optionen',lg:'Sprache',bk:'Zurück',dn:'Fertig',ws:'Welt auswählen',nw:'Noch keine Welten. Erstelle eine neue!',ps:'Ausgewählte Welt spielen',cn:'Neue Welt erstellen',de:'Löschen',dc:'Nochmal drücken zum Löschen!',sv:'Überleben',cr:'Kreativ',gm:'Spielmodus: {0}',wn:'Weltname',sd:'Seed (leer = zufällig)',ca:'Abbrechen',mt:'Erstellt Welten mit dem GLEICHEN Seed und öffnet diese Seite gleichzeitig, dann seht ihr euch.',rd:'Sichtweite: {0}',fv:'Sichtfeld: {0}',sn:'Empfindlichkeit: {0}%',so:'Ton: {0}',on:'AN',of:'AUS',ft:'Fan-Spiel, nicht mit Mojang oder Microsoft verbunden.',ld:'Gelände wird erzeugt...',inf:'Tippen: setzen/essen/schlagen | Halten: abbauen | ...: Menü',mn:'Menü',cl:'Schließen',nm:'Name:',bm:'Blöcke: {0} | Kreaturen in der Nähe: {1}',po:'{0} Spieler online',al:'Solo (Seite teilen, um zusammen zu spielen)',ch:'Figur: {0}',vw:'Ansicht: {0}',v1:'Ego-Perspektive',v3:'3. Person',sw:'Welt speichern',ex:'Speichern und zum Titel',hb:'Schnellleiste (Slot antippen, dann Block wählen)',iv:'Inventar',ai:'Alle Blöcke & Items',ih:'Blaue Reihe = Schnellleiste. Antippen zum Wählen, unten antippen zum Tauschen. Essen halten und tippen zum Essen.',cf:'Herstellen',cb:'Herstellen',hc:'KREATIV',fl:'fliegt',he:'Leben',hu:'Hunger',sa:'Welt gespeichert',sf:'Speichern nicht möglich',di:'Du bist gestorben! Zurück am Start',np:'Du brauchst eine bessere Spitzhacke!',bo:'BUMM!',ns:'Speicher blockiert, Welt kann nicht erstellt werden.',nwn:'Neue Welt'},
pt:{pl:'Um jogador',mp:'Multijogador',op:'Opções',lg:'Idioma',bk:'Voltar',dn:'Concluído',ws:'Selecionar mundo',nw:'Nenhum mundo ainda. Crie um novo!',ps:'Jogar mundo selecionado',cn:'Criar novo mundo',de:'Excluir',dc:'Toque de novo para excluir!',sv:'Sobrevivência',cr:'Criativo',gm:'Modo de jogo: {0}',wn:'Nome do mundo',sd:'Semente (vazio = aleatória)',ca:'Cance
