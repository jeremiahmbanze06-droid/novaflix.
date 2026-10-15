
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NovaFlix | Your World of Entertainment</title>
<style>
:root{--red:#e50914;--bg:#090909;--panel:#171717;--muted:#aaa}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:#fff;font-family:Arial,Helvetica,sans-serif}
header{height:72px;padding:0 5%;display:flex;align-items:center;gap:30px;background:#090909;position:sticky;top:0;z-index:5;border-bottom:1px solid #252525}
.logo{font-size:27px;font-weight:900;color:var(--red);letter-spacing:-1px}
nav{display:flex;gap:20px;flex:1}
nav a{color:#ddd;text-decoration:none;font-size:14px;cursor:pointer}
nav a:hover{color:var(--red)}
.search{width:210px;background:#1c1c1c;border:1px solid #444;border-radius:5px;padding:10px;color:white}
.hero{min-height:440px;padding:95px 6% 60px;background:linear-gradient(90deg,#080808 5%,rgba(8,8,8,.88) 38%,rgba(8,8,8,.1)),linear-gradient(0deg,#090909,transparent 55%),url('https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?auto=format&fit=crop&w=1800&q=85') center/cover}
.tag{color:#ff6068;font-size:12px;font-weight:bold;letter-spacing:3px}
.hero h1{font-size:clamp(38px,6vw,66px);max-width:600px;margin:15px 0}
.hero p{max-width:500px;line-height:1.7;color:#ddd}
button{cursor:pointer;border:0;border-radius:5px;font-weight:bold;padding:12px 19px}
.play{background:var(--red);color:white}.more{background:#353535;color:white;margin-left:8px}
main{padding:10px 5% 55px}
.toolbar{display:flex;align-items:center;justify-content:space-between;gap:15px;flex-wrap:wrap;margin:20px 0}
h2{font-size:21px;margin:25px 0 15px}
.filters{display:flex;gap:8px;flex-wrap:wrap}
.filters button{background:#222;color:white;padding:9px 13px}
.filters button.active,.filters button:hover{background:var(--red)}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(155px,1fr));gap:19px}
.movie{min-width:0;cursor:pointer;transition:transform .2s}
.movie:hover{transform:translateY(-5px)}
.poster{height:225px;border-radius:7px;background:#222 center/cover;position:relative;overflow:hidden;border:1px solid #303030}
.poster:after{content:"▶";position:absolute;inset:0;display:grid;place-items:center;background:#0008;color:white;font-size:35px;opacity:0;transition:.2s}
.movie:hover .poster:after{opacity:1}
.movie h3{font-size:14px;margin:10px 0 5px}
.meta{font-size:12px;color:var(--muted)}
.note{color:var(--muted);font-size:13px;line-height:1.6}
footer{padding:30px 5%;border-top:1px solid #282828;color:#888;font-size:13px}
.modal{display:none;position:fixed;inset:0;z-index:10;background:#000e;align-items:center;justify-content:center;padding:18px}
.modal.open{display:flex}
.dialog{background:#151515;border:1px solid #333;border-radius:10px;width:min(800px,100%);max-height:92vh;overflow:auto;padding:20px}
.dialog-top{display:flex;justify-content:space-between;align-items:center;gap:12px}
.close{background:#333;color:white;font-size:18px}
video{width:100%;background:#000;margin-top:15px;max-height:55vh}
#playerMessage{color:#ddd;font-size:14px;line-height:1.6}
@media(max-width:650px){header{height:auto;min-height:65px;flex-wrap:wrap;padding:15px 4%;gap:12px}nav{order:3;width:100%;flex-basis:100%;overflow:auto;padding:5px 0}.search{width:145px;margin-left:auto}.hero{padding:65px 5% 45px;min-height:390px}.grid{grid-template-columns:repeat(2,minmax(0,1fr));gap:13px}.poster{height:235px}main{padding-left:4%;padding-right:4%}}
</style>
</head>
<body>
<header>
  <div class="logo">NOVAFLIX</div>
  <nav>
    <a onclick="setCategory('All')">Home</a>
    <a onclick="setCategory('Movie')">Movies</a>
    <a onclick="setCategory('Series')">Series</a>
    <a onclick="setCategory('Anime')">Anime</a>
    <a href="#catalog">My Discoveries</a>
  </nav>
  <input class="search" id="search" placeholder="Search titles..." aria-label="Search movies" oninput="render()">
</header>

<section class="hero">
  <div class="tag">YOUR NEXT FAVORITE STARTS HERE</div>
  <h1>Entertainment without limits.</h1>
  <p>Discover exciting stories, explore new worlds, and find something worth watching. Your next movie night starts on NovaFlix.</p>
  <button class="play" onclick="openPlayer('Big Buck Bunny','https://storage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4')">▶ Watch Demo</button>
  <button class="more" onclick="document.getElementById('catalog').scrollIntoView({behavior:'smooth'})">Explore Titles</button>
</section>

<main id="catalog">
  <div class="toolbar">
    <div><h2 style="margin:0">Explore NovaFlix</h2><div class="note">Discover the catalog</div></div>
    <div class="filters" id="filters">
      <button class="active" onclick="setCategory('All',this)">All</button>
      <button onclick="setCategory('Movie',this)">Movies</button>
      <button onclick="setCategory('Series',this)">Series</button>
      <button onclick="setCategory('Anime',this)">Anime</button>
      <button onclick="setCategory('Demo',this)">Playable demos</button>
    </div>
  </div>
  <div class="note" id="count"></div>
  <div class="grid" id="grid"></div>
  <p class="note">Demo catalog: some entries are discovery examples, not playable films. Only titles with an available authorized video source can play. This prototype does not include a commercial movie library.</p>
</main>

<footer>
  <div class="logo">NOVAFLIX</div>
  <p>Discover your next favorite story.</p>
  <p>Demo website • Video playback depends on the availability and permission of each source.</p>
</footer>

<div class="modal" id="modal" onclick="if(event.target===this)closePlayer()">
  <div class="dialog">
    <div class="dialog-top"><h2 id="playerTitle" style="margin:0">Now Playing</h2><button class="close" onclick="closePlayer()">✕ Close</button></div>
    <video id="video" controls playsinline></video>
    <p id="playerMessage">Use the player controls to watch the demo.</p>
    <button class="more" onclick="closePlayer()">Back to NovaFlix</button>
  </div>
</div>

<script>
const titles=[
 {name:"Big Buck Bunny",type:"Movie",year:"2008",genre:"Animation",poster:"https://storage.googleapis.com/gtv-videos-bucket/sample/images/BigBuckBunny.jpg",video:"https://storage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4",desc:"A cheerful animated short film. This public sample demonstrates video playback."},
 {name:"Sintel",type:"Movie",year:"2010",genre:"Fantasy",poster:"https://storage.googleapis.com/gtv-videos-bucket/sample/images/Sintel.jpg",video:"https://storage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4",desc:"An animated fantasy short used here as a video demo."},
 {name:"Tears of Steel",type:"Movie",year:"2012",genre:"Science Fiction",poster:"https://storage.googleapis.com/gtv-videos-bucket/sample/images/TearsOfSteel.jpg",video:"https://storage.googleapis.com/gtv-videos-bucket/sample/TearsOfSteel.mp4",desc:"A science-fiction short used to test the NovaFlix player."},
 {name:"Elephant Dream",type:"Movie",year:"2006",genre:"Animation",poster:"https://storage.googleapis.com/gtv-videos-bucket/sample/images/ElephantsDream.jpg",video:"https://storage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4",desc:"An experimental animated short film."},
 {name:"For Bigger Escapes",type:"Movie",year:"Demo",genre:"Adventure",poster:"https://images.unsplash.com/photo-1478720568477-152d9b164e26?auto=format&fit=crop&w=500&q=80",video:"https://storage.googleapis.com/gtv-videos-bucket/sample/ForBiggerEscapes.mp4",desc:"A sample clip for checking video playback."},
 {name:"The Last Horizon",type:"Movie",year:"2025",genre:"Science Fiction",poster:"https://images.unsplash.com/photo-1446776811953-b23d57bd21aa?auto=format&fit=crop&w=500&q=80",desc:"A sample catalog entry. No video source is connected yet."},
 {name:"Midnight City",type:"Series",year:"2025",genre:"Drama",poster:"https://images.unsplash.com/photo-1519608487953-e999c86e7455?auto=format&fit=crop&w=500&q=80",desc:"A sample series listing. Episodes need an authorized source."},
 {name:"Ocean Secrets",type:"Series",year:"2024",genre:"Adventure",poster:"https://images.unsplash.com/photo-1518837695005-2083093ee35b?auto=format&fit=crop&w=500&q=80",desc:"A sample series listing. No episode videos are connected yet."},
 {name:"Star Warriors",type:"Anime",year:"2025",genre:"Anime",poster:"https://images.unsplash.com/photo-1446776811953-b23d57bd21aa?auto=format&fit=crop&w=500&q=80",desc:"A sample anime listing. No episode source is connected yet."},
 {name:"Dream Quest",type:"Anime",year:"2024",genre:"Fantasy",poster:"https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=500&q=80",desc:"A sample anime listing. Add an authorized episode source to enable playback."}
];
let category="All";
function setCategory(c,button){
 category=c;
 document.querySelectorAll("#filters button").forEach(b=>b.classList.toggle("active",b===button));
 if(!button)document.querySelectorAll("#filters button").forEach(b=>b.classList.toggle("active",b.textContent.trim()==={"All":"All","Movie":"Movies","Series":"Series","Anime":"Anime","Demo":"Playable demos"}[c]));
 render();
}
function render(){
 const q=document.getElementById("search").value.toLowerCase().trim();
 const filtered=titles.filter(t=>(category==="All"||(category==="Demo"?!!t.video:t.type===category))&&[t.name,t.type,t.genre,t.year].join(" ").toLowerCase().includes(q));
 document.getElementById("count").textContent=filtered.length+" titles";
 document.getElementById("grid").innerHTML=filtered.map(t=>`
 <article class="movie" tabindex="0" onclick="details('${encodeURIComponent(t.name)}')">
 <div class="poster" style="background-image:url('${t.poster}')"></div>
 <h3>${t.name}</h3><div class="meta">${t.year} · ${t.type} · ${t.genre}</div>
 </article>`).join("")||'<p class="note">No titles found. Try another search.</p>';
}
function details(encoded){
 const t=titles.find(x=>x.name===decodeURIComponent(encoded));if(!t)return;
 document.getElementById("playerTitle").textContent=t.name;
 document.getElementById("playerMessage").textContent=t.desc;
 const v=document.getElementById("video");v.pause();v.removeAttribute("src");v.load();
 if(t.video){
   document.getElementById("playerMessage").textContent=t.desc+" If playback fails, the sample source may be unavailable.";
   v.src=t.video;
   document.getElementById("modal").classList.add("open");
 }else{
   document.getElementById("playerMessage").textContent=t.desc+" This title is a catalog demo and cannot play until an authorized video source is connected.";
   document.getElementById("modal").classList.add("open");
 }
}
function openPlayer(name,url){
 document.getElementById("playerTitle").textContent=name;
 document.getElementById("playerMessage").textContent="Demo video. Playback depends on the sample source being available.";
 const v=document.getElementById("video");v.pause();v.src=url;
 document.getElementById("modal").classList.add("open");
 v.play().catch(()=>{document.getElementById("playerMessage").textContent="Press Play to start. If it still fails, the demo source may be unavailable."});
}
function closePlayer(){
 const v=document.getElementById("video");v.pause();v.removeAttribute("src");v.load();
 document.getElementById("modal").classList.remove("open");
}
document.addEventListener("keydown",e=>{if(e.key==="Escape")closePlayer()});
render();
</script>
</body>
</html>
