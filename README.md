[index.html](https://github.com/user-attachments/files/26165182/index.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="JurisCards">
<meta name="theme-color" content="#08061a">
<title>JurisCards</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>⚡</text></svg>">
<script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<style>
html,body{margin:0;padding:0;background:#08061a;overflow-x:hidden;-webkit-tap-highlight-color:transparent}
body{padding-top:env(safe-area-inset-top);padding-bottom:env(safe-area-inset-bottom)}
</style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const {useState,useEffect,useCallback,useMemo,useRef}=React;

const CSS = `
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700;800;900&family=JetBrains+Mono:wght@400;600&display=swap');
:root{--bg:#08061a;--bg2:#0f0c24;--bg3:#16123a;--accent:#7c5cfc;--accent2:#a78bfa;--green:#22c55e;--red:#ef4444;--orange:#f59e0b;--blue:#3b82f6;--text:#e8e5f8;--text2:#a09cc0;--text3:#6b6590;--gold:#fbbf24;--xp:#22d3ee}
*{box-sizing:border-box;margin:0;padding:0}body{background:var(--bg)}
::-webkit-scrollbar{width:5px}::-webkit-scrollbar-thumb{background:#534AB7;border-radius:9px}
input:focus,textarea:focus,select:focus{outline:none;border-color:var(--accent)!important;box-shadow:0 0 0 2px rgba(124,92,252,.15)}
@media(min-width:541px){body{display:flex;justify-content:center;min-height:100vh}
  .ce{animation:fadeUp .35s cubic-bezier(.22,1,.36,1)}
}
@media(max-width:380px){
  :root{font-size:14px}
}
@media(hover:hover){
  button:hover{opacity:.85}
}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}
@keyframes slideToast{from{opacity:0;transform:translate(-50%,-20px)}to{opacity:1;transform:translate(-50%,0)}}
@keyframes xpPop{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-40px) scale(1.3)}}
@keyframes comboShake{0%,100%{transform:translateX(0)}25%{transform:translateX(-3px)}75%{transform:translateX(3px)}}
@keyframes glow{0%,100%{box-shadow:0 0 8px rgba(124,92,252,.3)}50%{box-shadow:0 0 20px rgba(124,92,252,.6)}}
@keyframes ringPulse{0%,100%{box-shadow:0 0 0 0 rgba(124,92,252,.4)}50%{box-shadow:0 0 0 6px rgba(124,92,252,0)}}
@keyframes timerPulse{0%,100%{opacity:1}50%{opacity:.4}}
@keyframes correctFlash{0%{box-shadow:0 0 0 0 rgba(34,197,94,.5)}50%{box-shadow:0 0 20px 4px rgba(34,197,94,.2)}100%{box-shadow:0 0 0 0 rgba(34,197,94,0)}}
@keyframes wrongShake{0%,100%{transform:translateX(0)}20%{transform:translateX(-6px)}40%{transform:translateX(6px)}60%{transform:translateX(-4px)}80%{transform:translateX(4px)}}
@keyframes popIn{0%{transform:scale(0) rotate(-20deg);opacity:0}60%{transform:scale(1.2) rotate(5deg)}100%{transform:scale(1) rotate(0);opacity:1}}
@keyframes streakBurn{0%{transform:scale(1)}50%{transform:scale(1.3)}100%{transform:scale(1)}}
@keyframes lvlUp{0%{transform:scale(1)}30%{transform:scale(1.4)}60%{transform:scale(.9)}100%{transform:scale(1)}}
.ce{animation:fadeUp .35s cubic-bezier(.22,1,.36,1)}.ta{animation:slideToast .35s ease}
.xf{animation:xpPop .9s ease forwards;pointer-events:none}.cs{animation:comboShake .15s ease}
.lvl-ring{animation:ringPulse 2s ease infinite}
.correct-flash{animation:correctFlash .5s ease}
.wrong-shake{animation:wrongShake .4s ease}
.pop-in{animation:popIn .4s cubic-bezier(.34,1.56,.64,1)}
.streak-burn{animation:streakBurn .3s ease}
.lvl-up{animation:lvlUp .5s cubic-bezier(.34,1.56,.64,1)}
.timer-pulse{animation:timerPulse .5s ease infinite}
`;

const XP_PER_LEVEL = 250;

/* ═══════ QUOTES ═══════ */
const QT=[[{t:"Tout ce que je sais, c'est que je ne sais rien.",a:"Socrate (Platon, Apologie)"},{t:"Je pense, donc je suis.",a:"Descartes, Discours de la méthode, 1637"},{t:"Le doute est le commencement de la sagesse.",a:"Aristote"},{t:"Science sans conscience n'est que ruine de l'âme.",a:"Rabelais, Pantagruel, 1532"}],[{t:"Dura lex, sed lex.",a:"Adage latin — La loi est dure, mais c'est la loi"},{t:"Nemo censetur ignorare legem.",a:"Nul n'est censé ignorer la loi"},{t:"Entre le fort et le faible, c'est la liberté qui opprime et la loi qui affranchit.",a:"Lacordaire, 52ᵉ conférence de Notre-Dame, 1848"},{t:"La liberté est le droit de faire tout ce que les lois permettent.",a:"Montesquieu, De l'esprit des lois, XI-3, 1748"},{t:"Ubi lex non distinguit, nec nos distinguere debemus.",a:"Là où la loi ne distingue pas, il ne faut pas distinguer"}],[{t:"Il n'est pas de vent favorable pour celui qui ne sait pas où il va.",a:"Sénèque, Lettres à Lucilius, LXXI"},{t:"L'homme est condamné à être libre.",a:"Sartre, L'Être et le Néant, 1943"},{t:"Il faut imaginer Sisyphe heureux.",a:"Camus, Le Mythe de Sisyphe, 1942"},{t:"Ce qui ne me tue pas me rend plus fort.",a:"Nietzsche, Le Crépuscule des idoles, 1888"},{t:"La vraie générosité envers l'avenir consiste à tout donner au présent.",a:"Camus, L'Homme révolté, 1951"}],[{t:"Les lois inutiles affaiblissent les lois nécessaires.",a:"Montesquieu, De l'esprit des lois, XXIX-16"},{t:"Specialia generalibus derogant.",a:"Les lois spéciales dérogent aux générales"},{t:"Res judicata pro veritate habetur.",a:"Ulpien — La chose jugée est tenue pour vérité"},{t:"Les lois ne sont pas de purs actes de puissance ; ce sont des actes de sagesse.",a:"Portalis, Discours préliminaire du Code civil, 1801"},{t:"In dubio pro reo.",a:"Dans le doute, en faveur de l'accusé"}],[{t:"La justice sans la force est impuissante ; la force sans la justice est tyrannique.",a:"Pascal, Pensées, 1670"},{t:"La loi est l'expression de la volonté générale.",a:"DDHC, article 6, 1789"},{t:"Le droit est trop humain pour prétendre à l'absolu de la ligne droite.",a:"Carbonnier, Flexible droit, 1969"},{t:"Nul ne peut se prévaloir de sa propre turpitude.",a:"Nemo auditur propriam turpitudinem allegans"},{t:"L'ignorance des lois n'est point une excuse.",a:"Principe général du droit"}]];
function getQuote(lv){const t=lv<=3?0:lv<=7?1:lv<=12?2:lv<=18?3:4;const p=QT[t];return p[(lv*7+3)%p.length]}

/* ═══════ LEVEL ═══════ */
const TC=[["#cd7f32","#daa06d"],["#9ca3af","#d1d5db"],["#f59e0b","#fbbf24"],["#60a5fa","#93c5fd"],["#a855f7","#c084fc"]];
const TN=["Bronze","Argent","Or","Diamant","Maître"];
function tIdx(lv){return lv<=3?0:lv<=7?1:lv<=12?2:lv<=18?3:4}

/* ═══════ SM-2 ═══════ */
function mkEngine(s){return{
  init(c){return{...c,interval:0,repetition:0,easeFactor:s.startingEase/100,dueDate:new Date().toISOString(),lastReview:null,reviewCount:0,lapses:0,phase:"new"}},
  grade(c,q){let{interval,repetition,easeFactor:ef,lapses,phase}=c;const now=new Date();
    if(q===1){
      const mins=["new","learning","relearning"].includes(phase)?s.againStep:s.lapseSteps;
      return{...c,interval:0,repetition:0,easeFactor:Math.round(Math.max(1.3,ef-.2)*100)/100,dueDate:new Date(+now+mins*6e4).toISOString(),lastReview:now.toISOString(),reviewCount:(c.reviewCount||0)+1,lapses:lapses+1,phase:"relearning"};
    }
    if(["new","learning","relearning"].includes(phase)){if(q===4||(q===3&&phase!=="new")){const d=q===4?s.easyInterval:s.graduatingInterval;return{...c,interval:d,repetition:1,easeFactor:Math.round(Math.min(3,q===4?ef+.15:ef)*100)/100,dueDate:new Date(+now+d*864e5).toISOString(),lastReview:now.toISOString(),reviewCount:(c.reviewCount||0)+1,lapses,phase:"review"}}const m=q===2?s.hardStep:s.goodStep;return{...c,interval:0,repetition:0,easeFactor:Math.round(ef*100)/100,dueDate:new Date(+now+m*6e4).toISOString(),lastReview:now.toISOString(),reviewCount:(c.reviewCount||0)+1,lapses,phase:"learning"}}
    if(q===2){interval=Math.max(interval+1,Math.round(interval*1.2));ef=Math.max(1.3,ef-.15)}else if(q===3)interval=Math.round(interval*ef);else{interval=Math.round(interval*ef*(s.easyBonus/100));ef=Math.min(3,ef+.15)}
    interval=Math.min(Math.max(1,interval),s.maxInterval);return{...c,interval,repetition:repetition+1,easeFactor:Math.round(ef*100)/100,dueDate:new Date(+now+interval*864e5).toISOString(),lastReview:now.toISOString(),reviewCount:(c.reviewCount||0)+1,lapses,phase:"review"}}}}

const DEFS={againStep:1,hardStep:6,goodStep:10,graduatingInterval:1,easyInterval:4,lapseSteps:10,startingEase:250,easyBonus:130,maxInterval:180,newCardsPerDay:30,masteryResetDays:7,inactivityResetDays:3,examMode:"timer",examTotalSec:120,examQuestionCount:20,examXpBonus:2};
const DEMO=[
  {id:"d1",deck:"Libertés fondamentales::Cours::Chapitre 1",type:"flashcard",front:"Quel article consacre l'autorité judiciaire comme gardienne de la liberté individuelle ?",back:"Article 66 : « Nul ne peut être arbitrairement détenu. L'autorité judiciaire, gardienne de la liberté individuelle, assure le respect de ce principe. »",tags:["constitution"]},
  {id:"d2",deck:"Libertés fondamentales::Cours::Chapitre 1",type:"cloze",text:"La {{QPC}} permet de contester la {{constitutionnalité}} d'une loi portant atteinte aux {{droits et libertés}} garantis par la Constitution.",tags:["QPC"]},
  {id:"d3",deck:"Libertés fondamentales::TD::TD 1",type:"mcq",question:"Quelle décision a consacré la liberté d'association comme PFRLR ?",correct:"DC 71-44 du 16 juillet 1971",wrong:["DC 73-51 du 27 décembre 1973","DC 82-141 du 27 juillet 1982","DC 2008-562 du 21 février 2008"],tags:["PFRLR"]},
  {id:"d4",deck:"Droit administratif::Cours",type:"open",question:"Distinction police administrative générale / spéciale.",answer:"PAG : ordre public (sécurité, tranquillité, salubrité – CE 1919 Labonne). PAS : texte spécifique, objet précis. Maire = PAG (L.2212-2 CGCT).",tags:["police"]},
  {id:"d5",deck:"Droit administratif::TD::TD 1",type:"flashcard",front:"Portée de CE, 1950, Dehaene ?",back:"Droit de grève des agents publics reconnu, mais le gouvernement peut réglementer pour la continuité du service public.",tags:["grève"]},
];
const PROMPT_TPL=`Tu es un FORMATEUR JSON pour l'app JurisCards. Ton SEUL rôle : prendre mes questions déjà rédigées et les convertir en JSON importable.

RÈGLES ABSOLUES :
- NE MODIFIE JAMAIS le texte de mes questions/réponses. Recopie FIDÈLEMENT, mot pour mot.
- CONSERVE tous les **mots en gras** tels quels dans le JSON (l'app les affiche en gras).
- Ne reformule rien. Ne crée aucune nouvelle question.

DÉTECTION AUTOMATIQUE DU TYPE :
- "flashcard" → Toute question suivie d'une réponse (Q: ... / A: ... ou R: ...)
- "cloze" → Phrase avec des termes entre {{doubles accolades}} OU si j'écris [CLOZE]. Tu places les {{accolades}} sur les termes-clés juridiques si elles ne sont pas déjà là. Seul ajout autorisé.
- "mcq" → Question avec plusieurs choix OU si j'écris [QCM]. Si je ne donne que la bonne réponse, génère 3 fausses plausibles (seule création autorisée).
- "open" → Question de réflexion/argumentation OU si j'écris [OPEN].
- En cas de doute → "flashcard" par défaut.

FORMAT JSON EXACT :
[
  {"id":"deck-001","deck":"DECK","type":"flashcard","front":"Ma question exacte","back":"Ma réponse exacte","tags":["thème"]},
  {"id":"deck-002","deck":"DECK","type":"cloze","text":"Ma phrase avec {{termes}} entre accolades.","tags":["thème"]},
  {"id":"deck-003","deck":"DECK","type":"mcq","question":"Ma question","correct":"Bonne réponse","wrong":["F1","F2","F3"],"tags":["thème"]},
  {"id":"deck-004","deck":"DECK","type":"open","question":"Ma question","answer":"Ma réponse","tags":["thème"]}
]

INSTRUCTIONS :
- Le champ "deck" : mets "DECK" partout (je choisirai le deck à l'import).
- IDs uniques : "deck-001", "deck-002", etc.
- Tags : déduis 1-2 tags du thème de chaque question.
- Réponds UNIQUEMENT avec le JSON brut. PAS de \`\`\`json, PAS de \`\`\`, PAS de texte avant/après. Juste le tableau JSON.

Mes questions :`;

function ld(k,f){try{const v=localStorage.getItem(k);return v?JSON.parse(v):f}catch{return f}}
function sv(k,v){try{localStorage.setItem(k,JSON.stringify(v))}catch(e){console.error(e)}}

function applyResets(cards,s,xp,la){const now=new Date();let nxp=xp;if(s.inactivityResetDays>0&&la&&(now-new Date(la))/864e5>=s.inactivityResetDays)nxp=Math.floor(xp/XP_PER_LEVEL)*XP_PER_LEVEL;if(s.masteryResetDays>0)cards=cards.map(c=>(c.repetition>=3&&c.easeFactor>=2&&c.lastReview&&(now-new Date(c.lastReview))/864e5>=s.masteryResetDays)?{...c,dueDate:now.toISOString(),phase:"review"}:c);return{cards,xp:nxp}}

function buildTree(cards){const stats={};cards.forEach(c=>{if(!stats[c.deck])stats[c.deck]={total:0,due:0,nw:0,mast:0};stats[c.deck].total++;if(new Date(c.dueDate)<=new Date())stats[c.deck].due++;if(c.phase==="new")stats[c.deck].nw++;if(c.repetition>=3&&c.easeFactor>=2)stats[c.deck].mast++});const all=new Set();Object.keys(stats).forEach(p=>{all.add(p);const pts=p.split("::");for(let i=1;i<pts.length;i++)all.add(pts.slice(0,i).join("::"))});const nodes={};[...all].sort().forEach(p=>{const s=stats[p]||{total:0,due:0,nw:0,mast:0};nodes[p]={path:p,name:p.split("::").pop(),...s,children:[],hasCards:!!stats[p]}});const roots=[];[...all].sort().forEach(p=>{const pts=p.split("::");if(pts.length===1){roots.push(nodes[p]);return}const pp=pts.slice(0,-1).join("::");if(nodes[pp])nodes[pp].children.push(nodes[p]);else roots.push(nodes[p])});function agg(n){n.children.forEach(ch=>{agg(ch);n.total+=ch.total;n.due+=ch.due;n.nw+=ch.nw;n.mast+=ch.mast})}roots.forEach(agg);return roots}
function getParentPaths(cards){const s=new Set();cards.forEach(c=>{const p=c.deck.split("::");for(let i=1;i<=p.length;i++)s.add(p.slice(0,i).join("::"))});return[...s].sort()}

/* ═══════ FEEDBACK OVERLAY ═══════ */
function Feedback({type}){
  if(!type)return null;
  if(type==="correct")return <div className="pop-in" style={{position:"fixed",top:"40%",left:"50%",transform:"translate(-50%,-50%)",zIndex:200,pointerEvents:"none",display:"flex",flexDirection:"column",alignItems:"center",gap:4}}>
    <div style={{width:56,height:56,borderRadius:"50%",background:"rgba(34,197,94,.15)",display:"flex",alignItems:"center",justifyContent:"center"}}><svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="var(--green)" strokeWidth="3" strokeLinecap="round"><polyline points="20 6 9 17 4 12"/></svg></div>
  </div>;
  if(type==="wrong")return <div className="pop-in" style={{position:"fixed",top:"40%",left:"50%",transform:"translate(-50%,-50%)",zIndex:200,pointerEvents:"none"}}>
    <div style={{width:56,height:56,borderRadius:"50%",background:"rgba(239,68,68,.15)",display:"flex",alignItems:"center",justifyContent:"center"}}><svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="var(--red)" strokeWidth="3" strokeLinecap="round"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg></div>
  </div>;
  return null;
}

/* ═══════ ICONS ═══════ */
const I={
  Home:()=><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 01-2 2H5a2 2 0 01-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>,
  Cards:()=><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><rect x="2" y="3" width="20" height="14" rx="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg>,
  Import:()=><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>,
  Gear:()=><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 00.33 1.82l.06.06a2 2 0 010 2.83 2 2 0 01-2.83 0l-.06-.06a1.65 1.65 0 00-1.82-.33 1.65 1.65 0 00-1 1.51V21a2 2 0 01-4 0v-.09A1.65 1.65 0 009 19.4a1.65 1.65 0 00-1.82.33l-.06.06a2 2 0 01-2.83-2.83l.06-.06A1.65 1.65 0 004.68 15a1.65 1.65 0 00-1.51-1H3a2 2 0 010-4h.09A1.65 1.65 0 004.6 9a1.65 1.65 0 00-.33-1.82l-.06-.06a2 2 0 112.83-2.83l.06.06A1.65 1.65 0 009 4.6a1.65 1.65 0 001-1.51V3a2 2 0 114 0v.09a1.65 1.65 0 001 1.51 1.65 1.65 0 001.82-.33l.06-.06a2 2 0 012.83 2.83l-.06.06A1.65 1.65 0 0019.4 9a1.65 1.65 0 001.51 1H21a2 2 0 010 4h-.09a1.65 1.65 0 00-1.51 1z"/></svg>,
  Play:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="currentColor"><polygon points="5 3 19 12 5 21"/></svg>,
  Back:()=><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" strokeLinejoin="round"><line x1="19" y1="12" x2="5" y2="12"/><polyline points="12 19 5 12 12 5"/></svg>,
  Plus:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>,
  Trash:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 01-2 2H7a2 2 0 01-2-2V6m3 0V4a2 2 0 012-2h4a2 2 0 012 2v2"/></svg>,
  Copy:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round"><rect x="9" y="9" width="13" height="13" rx="2"/><path d="M5 15H4a2 2 0 01-2-2V4a2 2 0 012-2h9a2 2 0 012 2v1"/></svg>,
  Check:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="3" strokeLinecap="round"><polyline points="20 6 9 17 4 12"/></svg>,
  Chev:({open})=><svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5" strokeLinecap="round" style={{transition:"transform .2s",transform:open?"rotate(90deg)":"rotate(0)"}}><polyline points="9 18 15 12 9 6"/></svg>,
  Export:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>,
  Timer:()=><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round"><circle cx="12" cy="13" r="8"/><path d="M12 9v4l2 2"/><path d="M9 1h6"/></svg>,
};

function LevelBadge({level,xpPct,size=38}){const[c1,c2]=TC[tIdx(level)];const r=size/2;const st=3;const cr=r-st;const ci=2*Math.PI*cr;const off=ci-(xpPct/100)*ci;
  return <div style={{position:"relative",width:size,height:size,flexShrink:0}}><svg width={size} height={size} style={{transform:"rotate(-90deg)"}}><circle cx={r} cy={r} r={cr} fill="none" stroke="var(--bg3)" strokeWidth={st}/><circle cx={r} cy={r} r={cr} fill="none" stroke={c1} strokeWidth={st} strokeDasharray={ci} strokeDashoffset={off} strokeLinecap="round" style={{transition:"stroke-dashoffset .6s cubic-bezier(.22,1,.36,1)"}}/></svg>
  <div style={{position:"absolute",inset:0,display:"flex",alignItems:"center",justifyContent:"center"}}><div className="lvl-ring" style={{width:size-10,height:size-10,borderRadius:"50%",background:`linear-gradient(135deg,${c1},${c2})`,display:"flex",alignItems:"center",justifyContent:"center",fontSize:size*.35,fontWeight:900,color:"#fff",textShadow:"0 1px 4px rgba(0,0,0,.3)"}}>{level}</div></div></div>}

/* ═══════ APP ═══════ */
function App(){
  const[view,setView]=useState("home");const[cards,setCards]=useState([]);const[settings,setSettings]=useState(DEFS);const[xp,setXp]=useState(0);const[streak,setStreak]=useState({current:0,lastDate:null,best:0});const[studyDeck,setStudyDeck]=useState(null);const[studyMode,setStudyMode]=useState("normal");const[loaded,setLoaded]=useState(false);const[toast,setToast]=useState(null);const[todayN,setTodayN]=useState(0);const[fb,setFb]=useState(null);const prevLevel=useRef(1);const[lvlAnim,setLvlAnim]=useState(false);const[recentDecks,setRecentDecks]=useState([]);

  useEffect(()=>{const c=ld("jc6-cards",null),st=ld("jc6-set",DEFS),x=ld("jc6-xp",0),sk=ld("jc6-str",{current:0,lastDate:null,best:0}),td=ld("jc6-td",{d:null,n:0}),la=ld("jc6-la",null),rc=ld("jc6-rc",[]);const eng=mkEngine(st);let ic=c||DEMO.map(x=>eng.init(x));const rs=applyResets(ic,st,x,la);ic=rs.cards;setCards(ic);sv("jc6-cards",ic);setXp(rs.xp);sv("jc6-xp",rs.xp);setSettings(st);setStreak(sk);setTodayN(td.d===new Date().toDateString()?td.n:0);sv("jc6-la",new Date().toISOString());prevLevel.current=Math.floor(rs.xp/XP_PER_LEVEL)+1;setRecentDecks(rc);setLoaded(true)},[]);

  const engine=useMemo(()=>mkEngine(settings),[settings]);
  const uc=useCallback(fn=>{setCards(p=>{const n=typeof fn==="function"?fn(p):fn;sv("jc6-cards",n);return n})},[]);
  const us=useCallback(s=>{setSettings(s);sv("jc6-set",s)},[]);
  const axp=useCallback(a=>{setXp(p=>{const n=p+a;sv("jc6-xp",n);sv("jc6-la",new Date().toISOString());const newLv=Math.floor(n/XP_PER_LEVEL)+1;if(newLv>prevLevel.current){prevLevel.current=newLv;setLvlAnim(true);setTimeout(()=>setLvlAnim(false),600)}return n})},[]);
  const ar=useCallback(()=>{setTodayN(p=>{const n=p+1;sv("jc6-td",{d:new Date().toDateString(),n});return n})},[]);
  const usk=useCallback(s=>{setStreak(s);sv("jc6-str",s)},[]);
  const t_=useCallback((m,t="ok")=>{setToast({m,t});setTimeout(()=>setToast(null),2500)},[]);
  const showFb=useCallback(type=>{setFb(type);setTimeout(()=>setFb(null),500)},[]);

  const tree=useMemo(()=>buildTree(cards),[cards]);
  const totalDue=useMemo(()=>cards.filter(c=>new Date(c.dueDate)<=new Date()).length,[cards]);
  const level=Math.floor(xp/XP_PER_LEVEL)+1;const xpPct=Math.round((xp%XP_PER_LEVEL)/XP_PER_LEVEL*100);
  const quote=useMemo(()=>getQuote(level),[level]);

  const startStudy=(p,mode="normal")=>{setStudyDeck(p);setStudyMode(mode);setView("study");const nr=[p,...recentDecks.filter(x=>x!==p)].slice(0,5);setRecentDecks(nr);sv("jc6-rc",nr)};
  const importCards=(json,deckOverride)=>{try{let clean=json.trim();if(clean.startsWith("```"))clean=clean.replace(/^```\w*\n?/,"").replace(/\n?```$/,"").trim();const p=JSON.parse(clean);if(!Array.isArray(p))throw new Error("Doit être un tableau []");const nc=p.map(c=>engine.init({...c,id:c.id||`i${Date.now()}-${Math.random().toString(36).slice(2,6)}`,deck:deckOverride||c.deck||"Sans deck"}));uc(prev=>{const ids=new Set(prev.map(x=>x.id));return[...prev,...nc.filter(c=>!ids.has(c.id))]});t_(`${p.length} carte(s) → ${(deckOverride||"deck JSON").split("::").pop()} ✓`);setView("decks")}catch(e){t_("Erreur : "+e.message,"err")}};
  const deleteDeck=path=>{uc(p=>p.filter(c=>c.deck!==path&&!c.deck.startsWith(path+"::")));t_("Supprimé")};
  const createDeck=(path,nc)=>{uc(p=>[...p,...nc.map(c=>engine.init(c))]);t_(`"${path.split("::").pop()}" créé`);setView("decks")};
  const exportDeck=(path)=>{
    const dc=path?cards.filter(c=>c.deck===path||c.deck.startsWith(path+"::")):cards;
    const clean=dc.map(({id,deck,type,front,back,text,question,correct,wrong,answer,tags})=>({id,deck,type,...(front!==undefined&&{front,back}),...(text!==undefined&&{text}),...(question!==undefined&&{question}),...(correct!==undefined&&{correct,wrong}),...(answer!==undefined&&{answer}),tags}));
    const j=JSON.stringify(clean,null,2);
    const name=path?path.replace(/::/g,"-").replace(/\s/g,"_"):"juriscards-tout";
    const b=new Blob([j],{type:"application/json"});const a=document.createElement("a");a.href=URL.createObjectURL(b);a.download=`${name}.json`;a.click();
    t_(`${dc.length} carte(s) exportée(s) ✓`);
  };

  if(!loaded)return <div style={{display:"flex",alignItems:"center",justifyContent:"center",height:"100vh",background:"var(--bg)",fontFamily:"Outfit"}}><style>{CSS}</style><div style={{width:36,height:36,border:"3px solid var(--bg3)",borderTop:"3px solid var(--accent)",borderRadius:"50%",animation:"spin .7s linear infinite"}}/></div>;

  const inStudy=view==="study";
  return <div style={{minHeight:"100vh",background:"var(--bg)",fontFamily:"Outfit,sans-serif",color:"var(--text)",maxWidth:540,margin:"0 auto",paddingBottom:inStudy?16:84,boxShadow:"0 0 60px rgba(0,0,0,.3)",borderLeft:"1px solid rgba(124,92,252,.04)",borderRight:"1px solid rgba(124,92,252,.04)"}}>
    <style>{CSS}</style>
    <Feedback type={fb}/>
    {toast&&<div className="ta" style={{position:"fixed",top:16,left:"50%",transform:"translateX(-50%)",padding:"10px 22px",borderRadius:12,background:toast.t==="err"?"var(--red)":"var(--accent)",color:"#fff",fontSize:13,fontWeight:600,zIndex:999,boxShadow:"0 8px 30px rgba(0,0,0,.4)"}}>{toast.m}</div>}

    <div style={{padding:"12px 16px 6px",display:"flex",alignItems:"center",gap:10}}>
      <div className={lvlAnim?"lvl-up":""}><LevelBadge level={level} xpPct={xpPct}/></div>
      <div style={{flex:1}}>
        <div style={{display:"flex",justifyContent:"space-between",alignItems:"baseline",marginBottom:2}}>
          <span style={{fontSize:11,fontWeight:700,color:TC[tIdx(level)][0]}}>{TN[tIdx(level)]}</span>
          <span style={{fontSize:10,fontWeight:700,color:TC[tIdx(level)][1]}}>{xp%XP_PER_LEVEL}/{XP_PER_LEVEL} XP</span>
        </div>
        <div style={{height:4,borderRadius:9,background:"var(--bg3)",overflow:"hidden"}}><div style={{height:"100%",borderRadius:9,background:`linear-gradient(90deg,${TC[tIdx(level)][0]},${TC[tIdx(level)][1]})`,width:`${xpPct}%`,transition:"width .5s cubic-bezier(.22,1,.36,1)"}}/></div>
      </div>
      {streak.current>0&&<span className={streak.current>0?"streak-burn":""} style={{color:"var(--orange)",fontSize:12,fontWeight:800}}>🔥{streak.current}</span>}
    </div>

    <main style={{padding:"8px 16px 0"}}>
      {view==="home"&&<Home {...{tree,totalDue,cards,streak,todayN,startStudy,setView,quote,level,exportDeck,recentDecks}}/>}
      {view==="decks"&&<DeckListView {...{tree,cards,startStudy,deleteDeck,setView}}/>}
      {view==="create"&&<CreateView cards={cards} onCreate={createDeck} goBack={()=>setView("decks")} showToast={t_}/>}
      {view==="import"&&<ImportView onImport={importCards} setView={setView} cards={cards}/>}
      {view==="prompt"&&<PromptView/>}
      {view==="settings"&&<SettingsView settings={settings} updateSettings={us}/>}
      {view==="study"&&<StudyView {...{deckPath:studyDeck,mode:studyMode,cards,engine,updateCards:uc,streak,updateStreak:usk,addXp:axp,addReview:ar,onExit:()=>setView("decks"),showToast:t_,settings,showFb}}/>}
    </main>

    {!inStudy&&<nav style={{position:"fixed",bottom:0,left:0,right:0,background:"rgba(8,6,26,.95)",backdropFilter:"blur(16px)",borderTop:"1px solid rgba(124,92,252,.08)",padding:"5px 0 8px",display:"flex",justifyContent:"space-around",maxWidth:540,margin:"0 auto",zIndex:100}}>
      {[["home","Accueil",I.Home],["decks","Decks",I.Cards],["import","Import",I.Import],["settings","Réglages",I.Gear]].map(([v,l,Ic])=><button key={v} onClick={()=>setView(v)} style={{background:"none",border:"none",color:view===v?"var(--accent)":"var(--text3)",display:"flex",flexDirection:"column",alignItems:"center",gap:2,fontSize:10,fontWeight:view===v?700:500,cursor:"pointer",padding:"4px 14px",fontFamily:"Outfit"}}><Ic/><span>{l}</span></button>)}
    </nav>}
  </div>;
}

/* ═══════ HOME ═══════ */
function Home({tree,totalDue,cards,streak,todayN,startStudy,setView,quote,level,exportDeck,recentDecks}){
  const mastered=cards.filter(c=>c.repetition>=3&&c.easeFactor>=2).length;
  const phases=useMemo(()=>{let n=0,l=0,r=0;cards.forEach(c=>{if(c.phase==="new")n++;else if(c.repetition>=3)r++;else l++});return[{l:"Nouvelles",n,c:"var(--blue)"},{l:"En cours",n:l,c:"var(--orange)"},{l:"Acquises",n:r,c:"var(--green)"}]},[cards]);
  const mx=Math.max(1,...phases.map(p=>p.n));

  // Collect all decks with due cards (flat list for quick access)
  const dueDecks=useMemo(()=>{
    const m={};
    cards.forEach(c=>{if(new Date(c.dueDate)<=new Date()){
      const root=c.deck.split("::")[0];
      if(!m[root])m[root]={path:root,name:root,due:0};
      m[root].due++;
      // Also track exact path
      if(!m[c.deck])m[c.deck]={path:c.deck,name:c.deck.split("::").pop(),due:0,full:c.deck};
      m[c.deck].due++;
    }});
    return Object.values(m).filter(d=>d.due>0).sort((a,b)=>b.due-a.due);
  },[cards]);

  return <div className="ce">
    <h1 style={{fontSize:22,fontWeight:800,marginBottom:3}}>Tableau de bord</h1>
    <div style={{color:"var(--text3)",fontSize:11,marginBottom:16,lineHeight:1.4}}><span style={{fontStyle:"italic"}}>« {quote.t} »</span><br/><span style={{fontWeight:600,color:"var(--text2)",fontSize:10}}>{quote.a}</span></div>

    <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 1fr",gap:8,marginBottom:12}}>
      {[{v:totalDue,l:"À revoir",c:"var(--orange)",e:"📋"},{v:todayN,l:"Aujourd'hui",c:"var(--green)",e:"✅"},{v:`${mastered}/${cards.length}`,l:"Maîtrisées",c:"var(--accent)",e:"⭐"}].map((s,i)=> <div key={i} style={{background:"var(--bg2)",borderRadius:12,padding:"12px 8px",textAlign:"center",border:"1px solid rgba(124,92,252,.05)"}}><div style={{fontSize:18,marginBottom:2}}>{s.e}</div><div style={{fontSize:18,fontWeight:800,color:s.c,lineHeight:1.2}}>{s.v}</div><div style={{fontSize:9,color:"var(--text3)",marginTop:2,fontWeight:600,textTransform:"uppercase",letterSpacing:.5}}>{s.l}</div></div>)}
    </div>

    {/* Quick study — deck list with due cards */}
    {dueDecks.length>0 && <div style={P}>
      <PT>Réviser maintenant</PT>
      {dueDecks.filter(d=>d.full).slice(0,8).map(d=> <div key={d.path} style={{display:"flex",alignItems:"center",justifyContent:"space-between",gap:8,marginBottom:8}}>
        <div style={{flex:1,minWidth:0}}>
          <div style={{fontSize:13,fontWeight:600,overflow:"hidden",textOverflow:"ellipsis",whiteSpace:"nowrap"}}>{d.full||d.path}</div>
          <div style={{fontSize:10,color:"var(--orange)",fontWeight:600}}>{d.due} carte{d.due>1?"s":""}</div>
        </div>
        <div style={{display:"flex",gap:4,flexShrink:0}}>
          <Btn onClick={()=>startStudy(d.path)} accent small><I.Play/> Go</Btn>
          <Btn onClick={()=>startStudy(d.path,"exam")} small ghost><I.Timer/></Btn>
        </div>
      </div>)}
    </div>}

    {totalDue===0&&cards.length>0&& <div style={{...P,textAlign:"center",padding:"24px 16px"}}><div style={{fontSize:32,marginBottom:6}}>✅</div><p style={{color:"var(--green)",fontSize:13,fontWeight:600}}>Tout est à jour !</p><p style={{color:"var(--text3)",fontSize:11,marginTop:4}}>Reviens plus tard ou importe de nouvelles cartes.</p></div>}

    <div style={P}><PT>Répartition</PT>{phases.map((p,i)=> <div key={i} style={{display:"flex",alignItems:"center",gap:8,marginBottom:i<2?8:0}}><span style={{width:65,fontSize:11,color:"var(--text2)",fontWeight:500}}>{p.l}</span><div style={{flex:1,height:20,borderRadius:6,background:"var(--bg3)",overflow:"hidden",position:"relative"}}><div style={{height:"100%",borderRadius:6,background:p.c,width:`${Math.max(3,(p.n/mx)*100)}%`,transition:"width .6s cubic-bezier(.22,1,.36,1)"}}/><span style={{position:"absolute",right:6,top:"50%",transform:"translateY(-50%)",fontSize:10,fontWeight:700,color:"#fff",textShadow:"0 1px 4px rgba(0,0,0,.6)"}}>{p.n}</span></div></div>)}</div>

    {(()=>{const seen=new Set();const unique=recentDecks.filter(p=>{if(seen.has(p))return false;seen.add(p);return cards.some(c=>c.deck===p||c.deck.startsWith(p+"::"))}).slice(0,5);if(!unique.length)return null;return <div style={P}><PT>Dernières révisions</PT>{unique.map(path=>{
      const dc=cards.filter(c=>c.deck===path||c.deck.startsWith(path+"::"));
      const due=dc.filter(c=>new Date(c.dueDate)<=new Date()).length;
      const mast=dc.filter(c=>c.repetition>=3&&c.easeFactor>=2).length;
      const pct=dc.length>0?Math.round((mast/dc.length)*100):0;
      return <div key={path} style={{marginBottom:8}}>
        <div style={{display:"flex",alignItems:"center",justifyContent:"space-between",gap:8}}>
          <div style={{flex:1,minWidth:0}}>
            <div style={{fontSize:12,fontWeight:600,overflow:"hidden",textOverflow:"ellipsis",whiteSpace:"nowrap"}}>{path.replace(/::/g," › ")}</div>
            <div style={{fontSize:10,color:due>0?"var(--orange)":"var(--green)",fontWeight:500}}>{due>0?`${due} due`:"✓ à jour"} · {dc.length}c</div>
          </div>
          <div style={{display:"flex",gap:4,flexShrink:0}}>
            {due>0&&<Btn onClick={()=>startStudy(path)} accent small><I.Play/></Btn>}
            <Btn onClick={()=>startStudy(path,"exam")} small ghost><I.Timer/></Btn>
          </div>
        </div>
        <div style={{marginTop:4,height:3,borderRadius:9,background:"var(--bg3)",overflow:"hidden"}}><div style={{height:"100%",borderRadius:9,background:"linear-gradient(90deg,var(--accent),var(--green))",width:`${pct}%`}}/></div>
      </div>;
    })}</div>})()}
    <ExportSection cards={cards} exportDeck={exportDeck}/>
  </div>;
}
function TreeProg({n,startStudy,dep}){const[open,setOpen]=useState(false);const pct=n.total>0?Math.round((n.mast/n.total)*100):0;return <div style={{marginBottom:4}}><div style={{display:"flex",alignItems:"center",gap:5,paddingLeft:dep*14,cursor:"pointer"}} onClick={()=>n.children.length?setOpen(!open):n.hasCards&&n.due>0&&startStudy(n.path)}>{n.children.length>0&&<span style={{lineHeight:0}}><I.Chev open={open}/></span>}<span style={{fontSize:12,fontWeight:600,flex:1,overflow:"hidden",textOverflow:"ellipsis",whiteSpace:"nowrap",color:n.hasCards?"var(--text)":"var(--text2)"}}>{n.name}</span><span style={{fontSize:10,color:n.due>0?"var(--orange)":"var(--green)",fontWeight:600,flexShrink:0}}>{n.due>0?n.due:"✓"}</span></div><div style={{marginLeft:dep*14+(n.children.length?17:0),marginTop:2,height:3,borderRadius:9,background:"var(--bg3)",overflow:"hidden"}}><div style={{height:"100%",borderRadius:9,background:"linear-gradient(90deg,var(--accent),var(--green))",width:`${pct}%`}}/></div>{open&&n.children.map(ch=><TreeProg key={ch.path} n={ch} startStudy={startStudy} dep={dep+1}/>)}</div>}

/* ═══════ DECKS ═══════ */
function DeckListView({tree,cards,startStudy,deleteDeck,setView}){return <div className="ce"><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:16}}><h1 style={{fontSize:22,fontWeight:800}}>Mes Decks</h1><div style={{display:"flex",gap:6}}><Btn onClick={()=>setView("create")} small accent><I.Plus/> Créer</Btn><Btn onClick={()=>setView("prompt")} small ghost>Prompt</Btn></div></div>{tree.length===0&&<Empty e="📚" t="Aucun deck."/>}{tree.map(n=><DeckNode key={n.path} n={n} dep={0} startStudy={startStudy} deleteDeck={deleteDeck}/>)}</div>}
function DeckNode({n,dep,startStudy,deleteDeck}){const[open,setOpen]=useState(false);const[del,setDel]=useState(false);const pct=n.total>0?Math.round((n.mast/n.total)*100):0;return <div style={{marginLeft:dep*10,marginBottom:6}}><div style={{...P,padding:11,marginBottom:0}}><div style={{display:"flex",alignItems:"center",gap:5}}>{n.children.length>0&&<span onClick={()=>setOpen(!open)} style={{cursor:"pointer",lineHeight:0}}><I.Chev open={open}/></span>}<div style={{flex:1,minWidth:0}}><div style={{fontWeight:700,fontSize:13,display:"flex",alignItems:"center",gap:5}}>{n.name}{n.children.length>0&&<span style={{fontSize:9,color:"var(--text3)"}}>({n.children.length})</span>}</div><div style={{fontSize:10,color:"var(--text3)",marginTop:2,display:"flex",gap:8}}><span>{n.total}c</span><span style={{color:"var(--orange)"}}>{n.due} due</span><span style={{color:"var(--green)"}}>{n.mast} ok</span></div></div><div style={{display:"flex",gap:4,flexShrink:0}}>{n.due>0&&<Btn onClick={()=>startStudy(n.path)} accent small><I.Play/></Btn>}{n.due>0&&<Btn onClick={()=>startStudy(n.path,"exam")} small ghost><I.Timer/></Btn>}{del?<><Btn onClick={()=>{deleteDeck(n.path);setDel(false)}} danger small>Suppr ?</Btn><Btn onClick={()=>setDel(false)} small>Non</Btn></>:<Btn onClick={()=>setDel(true)} small ghost style={{color:"var(--red)"}}><I.Trash/></Btn>}</div></div><div style={{marginTop:5,height:3,borderRadius:9,background:"var(--bg3)",overflow:"hidden"}}><div style={{height:"100%",borderRadius:9,background:"linear-gradient(90deg,var(--accent),var(--green))",width:`${pct}%`}}/></div></div>{open&&n.children.map(ch=><DeckNode key={ch.path} n={ch} dep={dep+1} startStudy={startStudy} deleteDeck={deleteDeck}/>)}</div>}

/* ═══════ CREATE ═══════ */
function CreateView({cards,onCreate,goBack,showToast}){const[name,setName]=useState("");const[parent,setParent]=useState("");const[isSub,setIsSub]=useState(false);const[ci,setCi]=useState([{type:"flashcard",front:"",back:""}]);const[err,setErr]=useState("");const parents=useMemo(()=>getParentPaths(cards),[cards]);const fullPath=isSub&&parent?`${parent}::${name.trim()}`:name.trim();const add=()=>setCi(p=>[...p,{type:"flashcard",front:"",back:""}]);const rm=i=>setCi(p=>p.filter((_,j)=>j!==i));const upd=(i,f,v)=>{setErr("");setCi(p=>p.map((c,j)=>j===i?{...c,[f]:v}:c))};const chT=(i,t)=>{const b={type:t};if(t==="flashcard")Object.assign(b,{front:"",back:""});else if(t==="cloze")Object.assign(b,{text:""});else if(t==="mcq")Object.assign(b,{question:"",correct:"",wrong:["","",""]});else Object.assign(b,{question:"",answer:""});setCi(p=>p.map((c,j)=>j===i?b:c))};
  const go=()=>{
    setErr("");
    if(!name.trim()){setErr("Donne un nom au deck");return}
    const valid=ci.filter(c=>{if(c.type==="flashcard")return c.front&&c.back;if(c.type==="cloze")return c.text?.includes("{{");if(c.type==="mcq")return c.question&&c.correct;return c.question&&c.answer});
    if(!valid.length){setErr("Remplis au moins une carte (ou utilise l'Import pour ajouter un JSON dans ce deck)");return}
    onCreate(fullPath,valid.map((c,i)=>({...c,id:`${name.replace(/\s/g,"").slice(0,6)}-${Date.now()}-${i}`,deck:fullPath,tags:[]})));
  };
  return <div className="ce"><div style={{display:"flex",alignItems:"center",gap:10,marginBottom:16}}><Btn onClick={goBack} small><I.Back/></Btn><h1 style={{fontSize:22,fontWeight:800}}>Créer un deck</h1></div><div style={{display:"flex",alignItems:"center",gap:8,marginBottom:10}}><button onClick={()=>setIsSub(!isSub)} style={{width:36,height:20,borderRadius:10,background:isSub?"var(--accent)":"var(--bg3)",border:"none",cursor:"pointer",position:"relative"}}><div style={{width:16,height:16,borderRadius:8,background:"#fff",position:"absolute",top:2,left:isSub?18:2,transition:"left .2s"}}/></button><span style={{fontSize:12,color:"var(--text2)"}}>Sous-deck</span></div>
    {isSub&&<div style={{marginBottom:10}}><label style={{fontSize:11,fontWeight:600,color:"var(--text2)",display:"block",marginBottom:3}}>Deck parent</label>{parents.length>0?<select value={parent} onChange={e=>setParent(e.target.value)} style={selS}><option value="">Choisir…</option>{parents.map(p=><option key={p} value={p}>{p}</option>)}</select>:<Inp value={parent} onChange={setParent} ph="Parent" mini/>}</div>}
    <Inp label={isSub?"Nom du sous-deck":"Nom du deck"} value={name} onChange={v=>{setName(v);setErr("")}} ph={isSub?"Ex: TD 1":"Ex: Libertés fondamentales"}/>{fullPath&&<p style={{fontSize:11,color:"var(--accent2)",marginBottom:10}}>→ {fullPath}</p>}
    {ci.map((c,i)=><div key={i} style={{...P,marginBottom:8,padding:11}}><div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:8}}><span style={{fontSize:11,fontWeight:700,color:"var(--text3)"}}>Carte {i+1}</span><div style={{display:"flex",gap:5,alignItems:"center"}}><select value={c.type} onChange={e=>chT(i,e.target.value)} style={selS}><option value="flashcard">Flashcard</option><option value="cloze">Texte à trou</option><option value="mcq">QCM</option><option value="open">Question ouverte</option></select>{ci.length>1&&<button onClick={()=>rm(i)} style={{background:"none",border:"none",color:"var(--red)",cursor:"pointer"}}><I.Trash/></button>}</div></div>
      {c.type==="flashcard"&&<><Inp mini label="Recto" value={c.front} onChange={v=>upd(i,"front",v)}/><Inp mini label="Verso" value={c.back} onChange={v=>upd(i,"back",v)}/></>}
      {c.type==="cloze"&&<Inp mini label="Texte ({{mots}})" value={c.text} onChange={v=>upd(i,"text",v)} multi ph="La {{liberté}} est…"/>}
      {c.type==="mcq"&&<><Inp mini label="Question" value={c.question} onChange={v=>upd(i,"question",v)}/><Inp mini label="Bonne" value={c.correct} onChange={v=>upd(i,"correct",v)}/>{(c.wrong||["","",""]).map((w,wi)=><Inp key={wi} mini label={`Fausse ${wi+1}`} value={w} onChange={v=>{const n=[...(c.wrong||["","",""])];n[wi]=v;upd(i,"wrong",n)}}/>)}</>}
      {c.type==="open"&&<><Inp mini label="Question" value={c.question} onChange={v=>upd(i,"question",v)}/><Inp mini label="Réponse" value={c.answer} onChange={v=>upd(i,"answer",v)} multi/></>}
    </div>)}<Btn onClick={add} small ghost style={{width:"100%",justifyContent:"center",marginBottom:10}}><I.Plus/> Carte</Btn>
    {err&&<p style={{color:"var(--red)",fontSize:12,fontWeight:600,marginBottom:10,textAlign:"center"}}>{err}</p>}
    <Btn onClick={go} accent style={{width:"100%",justifyContent:"center"}}>Créer ({ci.length})</Btn>
  </div>;
}

/* ═══════ IMPORT / PROMPT ═══════ */
function ImportView({onImport,setView,cards}){
  const[j,setJ]=useState("");
  const[pickDeck,setPickDeck]=useState("");
  const[newDeck,setNewDeck]=useState("");
  const[err,setErr]=useState("");
  const allPaths=useMemo(()=>getParentPaths(cards),[cards]);
  const target=pickDeck==="__new"?newDeck.trim():pickDeck;

  const doImport=()=>{
    if(!target){setErr("Choisis un deck de destination");return}
    setErr("");
    onImport(j,target);
  };

  return <div className="ce">
    <h1 style={{fontSize:22,fontWeight:800,marginBottom:2}}>Importer</h1>
    <p style={{color:"var(--text3)",fontSize:12,marginBottom:14}}>Colle le JSON formaté. <span style={{color:"var(--accent)",cursor:"pointer",fontWeight:600}} onClick={()=>setView("prompt")}>Prompt formateur →</span></p>

    <div style={{...P,padding:12,marginBottom:10}}>
      <div style={{fontSize:11,fontWeight:700,color:"var(--text3)",marginBottom:8,textTransform:"uppercase",letterSpacing:.8}}>Destination du deck</div>
      <select value={pickDeck} onChange={e=>{setPickDeck(e.target.value);setErr("")}} style={selS}>
        <option value="">— Choisir le deck —</option>
        {allPaths.map(p=> <option key={p} value={p}>{p.replace(/::/g," › ")}</option>)}
        <option value="__new">+ Nouveau deck…</option>
      </select>
      {pickDeck==="__new"&& <div style={{marginTop:8}}><Inp value={newDeck} onChange={v=>{setNewDeck(v);setErr("")}} ph="Matière::Chapitre::Section" mini label="Chemin du nouveau deck (séparateur ::)"/></div>}
      {target&& <p style={{fontSize:11,color:"var(--accent2)",marginTop:6}}>→ {target.replace(/::/g," › ")}</p>}
    </div>

    <textarea value={j} onChange={e=>setJ(e.target.value)} rows={10} placeholder='[{"id":"1","type":"flashcard","front":"Q","back":"R","tags":["t"]}]' style={taS}/>
    {err&&<p style={{color:"var(--red)",fontSize:12,fontWeight:600,marginTop:6,textAlign:"center"}}>{err}</p>}
    <div style={{display:"flex",gap:8,marginTop:10}}>
      <Btn onClick={doImport} accent style={{flex:1,justifyContent:"center"}}>{target?"Importer → "+target.split("::").pop():"Importer"}</Btn>
      <Btn onClick={()=>setJ(`[\n  {"id":"ex-1","type":"flashcard","front":"Q ?","back":"R.","tags":["t"]}\n]`)} small ghost>Ex.</Btn>
    </div>
  </div>;
}
function PromptView(){const[cp,setCp]=useState(false);return <div className="ce"><h1 style={{fontSize:22,fontWeight:800,marginBottom:2}}>Prompt de formatage</h1><p style={{color:"var(--text3)",fontSize:12,marginBottom:14}}>Copie → Claude + tes questions → JSON → importe.</p><div style={{position:"relative"}}><pre style={{background:"var(--bg2)",borderRadius:12,padding:14,fontSize:10,fontFamily:"JetBrains Mono",color:"var(--text2)",lineHeight:1.5,overflow:"auto",maxHeight:360,border:"1px solid rgba(124,92,252,.06)",whiteSpace:"pre-wrap",wordBreak:"break-word"}}>{PROMPT_TPL}</pre><button onClick={()=>{navigator.clipboard.writeText(PROMPT_TPL).catch(()=>{});setCp(true);setTimeout(()=>setCp(false),2e3)}} style={{position:"absolute",top:8,right:8,background:cp?"var(--green)":"var(--accent)",border:"none",borderRadius:8,padding:"5px 10px",color:"#fff",fontSize:10,fontWeight:600,cursor:"pointer",display:"flex",alignItems:"center",gap:4,fontFamily:"Outfit"}}>{cp?<><I.Check/> Copié</>:<><I.Copy/> Copier</>}</button></div></div>}

/* ═══════ SETTINGS ═══════ */
function SettingsView({settings,updateSettings}){const[s,setS]=useState({...settings});const changed=JSON.stringify(s)!==JSON.stringify(settings);const set=(k,v)=>setS(p=>({...p,[k]:v}));
  const groups=[
    {t:"🎯 Apprentissage",d:"Délai (min) entre les révisions d'une carte en cours d'acquisition",f:[{k:"againStep",l:"À revoir",u:"m",d:"Pas du tout connue → courte pause"},{k:"hardStep",l:"Difficile",u:"m",d:"Hésitation → pause moyenne"},{k:"goodStep",l:"Bien",u:"m",d:"Connue → pause plus longue"}]},
    {t:"📈 Passage en révision",d:"Quand la carte est bien sue, elle passe en jours au lieu de minutes",f:[{k:"graduatingInterval",l:"Après « Bien »",u:"j",d:"1ᵉʳ intervalle en jours"},{k:"easyInterval",l:"Après « Facile »",u:"j",d:"Saut direct en révision longue"}]},
    {t:"💀 Oubli",d:"Carte acquise puis oubliée → retour en apprentissage",f:[{k:"lapseSteps",l:"Réapprentissage",u:"m",d:"Délai avant de revoir la carte oubliée"}]},
    {t:"🔄 Reset",d:"Maîtrise temporaire et pénalité d'inactivité",f:[{k:"masteryResetDays",l:"Maîtrise expire",u:"j",d:"Carte acquise redevient due (0 = jamais)"},{k:"inactivityResetDays",l:"XP decay",u:"j",d:"Inactif → perte XP du niveau (0 = off)"}]},
    {t:"⚙️ SM-2",d:"Moteur avancé de répétition espacée",f:[{k:"startingEase",l:"Facilité init.",u:"%",d:"Multiplicateur de départ (250 = ×2.5)"},{k:"easyBonus",l:"Bonus facile",u:"%",d:"Extra pour réponse Facile"},{k:"maxInterval",l:"Max intervalle",u:"j",d:"Plafond entre 2 révisions"},{k:"newCardsPerDay",l:"Nouvelles/jour",u:"",d:"Limite nouvelles cartes par session"}]},
  ];
  return <div className="ce"><h1 style={{fontSize:22,fontWeight:800,marginBottom:16}}>Réglages</h1>
    {groups.map((g,gi)=> <div key={gi} style={{...P,marginBottom:10}}><div style={{fontSize:13,fontWeight:700,color:"var(--accent2)",marginBottom:2}}>{g.t}</div><div style={{fontSize:10,color:"var(--text3)",marginBottom:10}}>{g.d}</div>
      {g.f.map((f,fi)=> <div key={f.k} style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:fi<g.f.length-1?10:0}}><div style={{flex:1,minWidth:0,marginRight:8}}><div style={{fontSize:12,fontWeight:600}}>{f.l}</div>{f.d&&<div style={{fontSize:9,color:"var(--text3)",marginTop:1}}>{f.d}</div>}</div><div style={{display:"flex",alignItems:"center",gap:4,flexShrink:0}}><input type="number" value={s[f.k]} onChange={e=>set(f.k,Math.max(0,parseInt(e.target.value)||0))} style={{width:56,background:"var(--bg3)",border:"1px solid rgba(124,92,252,.12)",borderRadius:7,color:"var(--text)",padding:"5px 6px",fontSize:13,fontWeight:700,textAlign:"right",fontFamily:"JetBrains Mono"}}/>{f.u&&<span style={{fontSize:10,color:"var(--text3)",minWidth:14}}>{f.u}</span>}</div></div>)}
    </div>)}

    {/* EXAM SETTINGS — custom section */}
    <div style={{...P,marginBottom:10}}>
      <div style={{fontSize:13,fontWeight:700,color:"var(--accent2)",marginBottom:2}}>⏱️ Mode examen</div>
      <div style={{fontSize:10,color:"var(--text3)",marginBottom:10}}>Test final indépendant — ne modifie pas les stats</div>

      <div style={{fontSize:12,fontWeight:600,marginBottom:6}}>Mode</div>
      <div style={{display:"flex",gap:6,marginBottom:12}}>
        {[["timer","⏱ Chrono","Temps limité"],["count","🔢 Nb questions","X questions aléatoires"]].map(([v,l,d])=> <button key={v} onClick={()=>set("examMode",v)} style={{flex:1,padding:"8px 4px",borderRadius:8,fontSize:11,fontWeight:600,fontFamily:"Outfit",cursor:"pointer",border:s.examMode===v?"1px solid var(--accent)":"1px solid rgba(124,92,252,.12)",background:s.examMode===v?"rgba(124,92,252,.12)":"transparent",color:s.examMode===v?"var(--accent)":"var(--text3)",textAlign:"center"}}>
          <div>{l}</div><div style={{fontSize:9,fontWeight:400,marginTop:2,opacity:.7}}>{d}</div>
        </button>)}
      </div>

      {s.examMode==="timer"&& <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:10}}>
        <div><div style={{fontSize:12,fontWeight:600}}>Durée totale</div><div style={{fontSize:9,color:"var(--text3)"}}>Temps pour tout l'examen</div></div>
        <div style={{display:"flex",alignItems:"center",gap:4}}><input type="number" value={s.examTotalSec} onChange={e=>set("examTotalSec",Math.max(10,parseInt(e.target.value)||60))} style={{width:56,background:"var(--bg3)",border:"1px solid rgba(124,92,252,.12)",borderRadius:7,color:"var(--text)",padding:"5px 6px",fontSize:13,fontWeight:700,textAlign:"right",fontFamily:"JetBrains Mono"}}/><span style={{fontSize:10,color:"var(--text3)"}}>s</span></div>
      </div>}

      {s.examMode==="count"&& <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:10}}>
        <div><div style={{fontSize:12,fontWeight:600}}>Nombre de questions</div><div style={{fontSize:9,color:"var(--text3)"}}>Tirées aléatoirement du deck</div></div>
        <div style={{display:"flex",alignItems:"center",gap:4}}><input type="number" value={s.examQuestionCount} onChange={e=>set("examQuestionCount",Math.max(1,parseInt(e.target.value)||10))} style={{width:56,background:"var(--bg3)",border:"1px solid rgba(124,92,252,.12)",borderRadius:7,color:"var(--text)",padding:"5px 6px",fontSize:13,fontWeight:700,textAlign:"right",fontFamily:"JetBrains Mono"}}/><span style={{fontSize:10,color:"var(--text3)"}}>q</span></div>
      </div>}

      <div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}>
        <div><div style={{fontSize:12,fontWeight:600}}>Bonus XP</div><div style={{fontSize:9,color:"var(--text3)"}}>Multiplicateur en examen</div></div>
        <div style={{display:"flex",alignItems:"center",gap:4}}><input type="number" value={s.examXpBonus} onChange={e=>set("examXpBonus",Math.max(1,parseInt(e.target.value)||1))} style={{width:56,background:"var(--bg3)",border:"1px solid rgba(124,92,252,.12)",borderRadius:7,color:"var(--text)",padding:"5px 6px",fontSize:13,fontWeight:700,textAlign:"right",fontFamily:"JetBrains Mono"}}/><span style={{fontSize:10,color:"var(--text3)"}}>×</span></div>
      </div>
    </div>

    <div style={{display:"flex",gap:8}}><Btn onClick={()=>updateSettings(s)} accent style={{flex:1,justifyContent:"center",opacity:changed?1:.4}}>Appliquer</Btn><Btn onClick={()=>setS({...DEFS})} small ghost>Défaut</Btn></div>
  </div>;
}

/* ═══════ STUDY ═══════ */
function StudyView({deckPath,mode,cards,engine,updateCards,streak,updateStreak,addXp,addReview,onExit,showToast,settings,showFb}){
  const isExam=mode==="exam";
  const examIsTimed=settings.examMode==="timer";
  const studyCards=useMemo(()=>{
    let dc=cards.filter(c=>c.deck===deckPath||c.deck.startsWith(deckPath+"::"));
    if(isExam){
      for(let i=dc.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[dc[i],dc[j]]=[dc[j],dc[i]]}
      if(!examIsTimed) dc=dc.slice(0,settings.examQuestionCount);
    } else {
      dc=dc.filter(c=>new Date(c.dueDate)<=new Date());
      dc.sort((a,b)=>({new:0,learning:1,relearning:2,review:3}[a.phase]||0)-({new:0,learning:1,relearning:2,review:3}[b.phase]||0));
    }
    return dc;
  },[cards,deckPath,isExam,examIsTimed,settings.examQuestionCount]);

  const[idx,setIdx]=useState(0);const[done,setDone]=useState(0);const[combo,setCombo]=useState(0);const[bestC,setBestC]=useState(0);
  const[xpG,setXpG]=useState(0);const[xpF,setXpF]=useState(null);const[fin,setFin]=useState(studyCards.length===0);
  const[examScore,setExamScore]=useState(0);const[examTotal,setExamTotal]=useState(0);
  const[globalTimer,setGlobalTimer]=useState(settings.examTotalSec);const timerRef=useRef(null);
  const card=studyCards[idx];
  const[cardAnim,setCardAnim]=useState("");

  // Global exam timer — only in timed mode
  useEffect(()=>{
    if(!isExam||!examIsTimed||fin)return;
    setGlobalTimer(settings.examTotalSec);
    timerRef.current=setInterval(()=>setGlobalTimer(t=>{if(t<=1){clearInterval(timerRef.current);setFin(true);return 0}return t-1}),1000);
    return()=>clearInterval(timerRef.current);
  },[isExam,examIsTimed,fin]);

  const grade=useCallback(q=>{
    if(!card)return;
    if(timerRef.current&&isExam&&fin)return;
    const good=q>=3;
    showFb(good?"correct":"wrong");
    setCardAnim(good?"correct-flash":"wrong-shake");
    setTimeout(()=>setCardAnim(""),500);

    if(isExam){
      if(good)setExamScore(p=>p+1);
      setExamTotal(p=>p+1);
      let xp=Math.round(5*settings.examXpBonus);
      if(good){const nc=combo+1;setCombo(nc);setBestC(p=>Math.max(p,nc));xp=Math.round(xp*Math.min(4,1+Math.floor(nc/3)*.5))}else{setCombo(0);xp=2}
      addXp(xp);setXpG(p=>p+xp);setXpF({id:Date.now(),a:xp,g:good});setTimeout(()=>setXpF(null),900);setDone(p=>p+1);
      if(idx+1>=studyCards.length){clearInterval(timerRef.current);setFin(true)}else setIdx(i=>i+1);
      return;
    }
    updateCards(p=>p.map(c=>c.id===card.id?engine.grade(card,q):c));addReview();
    let xp=5;if(q>=3){const nc=combo+1;setCombo(nc);setBestC(p=>Math.max(p,nc));xp=Math.round(xp*Math.min(4,1+Math.floor(nc/3)*.5));if(q===4)xp+=5}else{setCombo(0);xp=2}
    addXp(xp);setXpG(p=>p+xp);setXpF({id:Date.now(),a:xp,g:q>=3});setTimeout(()=>setXpF(null),900);setDone(p=>p+1);
    const today=new Date().toDateString();if(streak.lastDate!==today){const y=new Date();y.setDate(y.getDate()-1);const nc=streak.lastDate===y.toDateString()?streak.current+1:1;updateStreak({current:nc,lastDate:today,best:Math.max(streak.best||0,nc)})}
    if(idx+1>=studyCards.length)setFin(true);else setIdx(i=>i+1);
  },[card,idx,studyCards.length,combo,isExam,fin,engine,updateCards,addReview,addXp,streak,updateStreak,settings,showFb]);

  const pv=q=>{if(!card||isExam)return"";if(q===1)return["new","learning","relearning"].includes(card.phase)?`${settings.againStep}m`:`${settings.lapseSteps}m`;if(["new","learning","relearning"].includes(card.phase)){if(q===2)return`${settings.hardStep}m`;if(q===3)return card.phase==="new"?`${settings.goodStep}m`:`${settings.graduatingInterval}j`;return`${settings.easyInterval}j`}const ef=card.easeFactor;if(q===2)return`${Math.max(card.interval+1,Math.round(card.interval*1.2))}j`;if(q===3)return`${Math.min(settings.maxInterval,Math.round(card.interval*ef))}j`;return`${Math.min(settings.maxInterval,Math.round(card.interval*ef*(settings.easyBonus/100)))}j`};

  useEffect(()=>{const h=e=>{
    if(isExam){if(e.key==="ArrowLeft"||e.key==="1")grade(1);if(e.key==="ArrowRight"||e.key==="2")grade(3)}
    else{const n=parseInt(e.key);if(n>=1&&n<=4)grade(n)}
  };window.addEventListener("keydown",h);return()=>window.removeEventListener("keydown",h)},[grade,isExam]);

  const fmtTime=s=>{const m=Math.floor(s/60);const sec=s%60;return`${m}:${sec.toString().padStart(2,"0")}`};

  if(fin||!card){const note=examTotal>0?Math.round((examScore/examTotal)*20):0;
    return <div className="ce" style={{textAlign:"center",padding:"40px 16px"}}>
      <div style={{fontSize:52,marginBottom:10}}>{done>0?(isExam?"📝":"🎉"):"✅"}</div>
      <h2 style={{fontSize:20,fontWeight:800,marginBottom:6}}>{isExam?(examIsTimed&&globalTimer<=0?"Temps écoulé !":"Examen terminé !"):done>0?"Session terminée !":"Tout est à jour !"}</h2>
      {isExam&&examTotal>0&&<div style={{margin:"16px auto",width:100,height:100,borderRadius:"50%",background:`conic-gradient(${note>=14?"var(--green)":note>=10?"var(--orange)":"var(--red)"} ${(note/20)*360}deg,var(--bg3) 0)`,display:"flex",alignItems:"center",justifyContent:"center"}}><div style={{width:80,height:80,borderRadius:"50%",background:"var(--bg)",display:"flex",alignItems:"center",justifyContent:"center",flexDirection:"column"}}><span style={{fontSize:28,fontWeight:900,color:note>=14?"var(--green)":note>=10?"var(--orange)":"var(--red)"}}>{note}</span><span style={{fontSize:11,color:"var(--text3)"}}>/20</span></div></div>}
      {done>0&&<div style={{display:"flex",justifyContent:"center",gap:20,margin:"16px 0"}}>{[{v:isExam?`${examScore}/${examTotal}`:done,l:isExam?"Bonnes":"Cartes",c:"var(--accent)"},{v:`+${xpG}`,l:"XP",c:"var(--xp)"},{v:`${bestC}×`,l:"Combo",c:"var(--orange)"}].map((s,i)=><div key={i}><div style={{fontSize:20,fontWeight:800,color:s.c}}>{s.v}</div><div style={{fontSize:10,color:"var(--text3)"}}>{s.l}</div></div>)}</div>}
      <Btn onClick={()=>{if(timerRef.current)clearInterval(timerRef.current);onExit()}} accent style={{margin:"16px auto 0"}}>Retour</Btn>
    </div>;
  }

  const pct=(idx/studyCards.length)*100;
  return <div>
    <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:8}}>
      <Btn onClick={()=>{if(timerRef.current)clearInterval(timerRef.current);onExit()}} small><I.Back/></Btn>
      <div style={{flex:1}}>
        <div style={{display:"flex",justifyContent:"space-between",marginBottom:3}}>
          <span style={{fontSize:10,color:"var(--text3)",overflow:"hidden",textOverflow:"ellipsis",whiteSpace:"nowrap",maxWidth:"50%"}}>{card.deck.split("::").pop()}</span>
          <div style={{display:"flex",alignItems:"center",gap:8}}>
            {isExam&&examIsTimed&&<span className={globalTimer<=10?"timer-pulse":""} style={{fontSize:13,fontWeight:800,color:globalTimer<=15?"var(--red)":"var(--orange)",fontFamily:"JetBrains Mono"}}>{fmtTime(globalTimer)}</span>}
            <span style={{fontSize:11,color:"var(--text2)",fontWeight:700}}>{idx+1}/{studyCards.length}</span>
          </div>
        </div>
        <div style={{height:4,borderRadius:9,background:"var(--bg3)",overflow:"hidden"}}><div style={{height:"100%",borderRadius:9,background:isExam?"linear-gradient(90deg,var(--red),var(--orange))":"linear-gradient(90deg,var(--accent),var(--green))",width:`${pct}%`,transition:"width .3s"}}/></div>
      </div>
    </div>
    {isExam&&<div style={{textAlign:"center",marginBottom:6}}><span style={{fontSize:10,fontWeight:700,color:"var(--red)",background:"rgba(239,68,68,.1)",padding:"2px 8px",borderRadius:6,textTransform:"uppercase",letterSpacing:1}}>{examIsTimed?`Examen — ${studyCards.length}q — ${fmtTime(globalTimer)}`:`Examen — ${idx+1}/${studyCards.length}`}</span></div>}
    {combo>0&&<div className="cs" style={{textAlign:"center",marginBottom:6}}><span style={{fontSize:12,fontWeight:800,color:combo>=9?"var(--gold)":combo>=6?"var(--orange)":"var(--accent2)"}}>{combo>=6?"🔥🔥":combo>=3?"🔥":"⚡"} ×{combo}</span></div>}
    {xpF&&<div className="xf" style={{textAlign:"center",color:xpF.g?"var(--xp)":"var(--text3)",fontSize:15,fontWeight:800}}>+{xpF.a} XP</div>}
    <div key={card.id} className={`ce ${cardAnim}`}>
      {card.type==="flashcard"&&<FlashV c={card}/>}
      {card.type==="cloze"&&<ClozeV c={card}/>}
      {card.type==="mcq"&&<McqV c={card}/>}
      {card.type==="open"&&<OpenV c={card}/>}
    </div>
    {isExam ? (
      <div style={{display:"flex",gap:8,marginTop:10}}>
        <button onClick={()=>grade(1)} style={{flex:1,padding:"12px 4px",borderRadius:10,background:"rgba(239,68,68,.1)",border:"1px solid rgba(239,68,68,.2)",color:"var(--red)",cursor:"pointer",fontFamily:"Outfit",fontSize:13,fontWeight:700,display:"flex",alignItems:"center",justifyContent:"center",gap:6}}>✗ Faux</button>
        <button onClick={()=>grade(3)} style={{flex:1,padding:"12px 4px",borderRadius:10,background:"rgba(34,197,94,.1)",border:"1px solid rgba(34,197,94,.2)",color:"var(--green)",cursor:"pointer",fontFamily:"Outfit",fontSize:13,fontWeight:700,display:"flex",alignItems:"center",justifyContent:"center",gap:6}}>✓ Correct</button>
      </div>
    ) : (
      <div style={{display:"flex",gap:5,marginTop:10}}>
        {[{q:1,l:"À revoir",c:"var(--red)"},{q:2,l:"Difficile",c:"var(--orange)"},{q:3,l:"Bien",c:"var(--blue)"},{q:4,l:"Facile",c:"var(--green)"}].map(b=><button key={b.q} onClick={()=>grade(b.q)} style={{flex:1,padding:"9px 2px",borderRadius:10,background:`color-mix(in srgb,${b.c} 10%,transparent)`,border:`1px solid color-mix(in srgb,${b.c} 20%,transparent)`,color:b.c,cursor:"pointer",fontFamily:"Outfit",display:"flex",flexDirection:"column",alignItems:"center",gap:1}}><span style={{fontSize:11,fontWeight:700}}>{b.l}</span><span style={{fontSize:9,opacity:.65}}>{pv(b.q)}</span></button>)}
      </div>
    )}
    <p style={{textAlign:"center",marginTop:6,fontSize:10,color:"var(--text3)"}}>{isExam?"":"1 2 3 4"}</p>
  </div>;
}

/* ═══════ RICH TEXT (renders **bold**) ═══════ */
function RT({children,style={}}){
  if(typeof children!=="string") return <p style={style}>{children}</p>;
  const parts=children.split(/(\*\*[^*]+\*\*)/g);
  return <p style={style}>{parts.map((p,i)=>p.startsWith("**")&&p.endsWith("**")? <strong key={i} style={{color:"var(--accent2)",fontWeight:700}}>{p.slice(2,-2)}</strong> : <span key={i}>{p}</span>)}</p>;
}

/* ═══════ CARDS ═══════ */
function FlashV({c}){const[f,setF]=useState(false);useEffect(()=>setF(false),[c.id]);
  return <div onClick={()=>setF(!f)} style={{cursor:"pointer",marginBottom:8}}>
    {!f ? <div style={{...fS,border:"1px solid rgba(124,92,252,.08)"}}><Bdg c="var(--accent)">Flashcard</Bdg><RT style={cT}>{c.front}</RT><p style={{color:"var(--text3)",fontSize:10,marginTop:12}}>Touche pour retourner ↻</p></div>
    : <div style={{...fS,border:"1px solid rgba(34,197,94,.12)",animation:"fadeUp .3s ease"}}><Bdg c="var(--green)">Réponse</Bdg><RT style={cT}>{c.back}</RT></div>}
  </div>;
}

function ClozeV({c}){const blanks=useMemo(()=>{const r=[];let m;const rx=/\{\{(.+?)\}\}/g;while((m=rx.exec(c.text)))r.push(m[1]);return r},[c.id]);const[a,setA]=useState([]);const[rv,setRv]=useState(false);useEffect(()=>{setA(blanks.map(()=>""));setRv(false)},[c.id]);const segs=c.text.split(/\{\{.+?\}\}/);return <div style={{...sC,marginBottom:8}}><Bdg c="var(--blue)">Texte à trou</Bdg><div style={{lineHeight:2.2,fontSize:13,color:"var(--text)",marginTop:10}}>{segs.map((seg,i)=><span key={i}>{seg}{i<blanks.length&&(rv?<span style={{display:"inline-block",padding:"1px 8px",borderRadius:5,fontWeight:700,margin:"0 2px",fontSize:12,background:a[i]?.trim().toLowerCase()===blanks[i].trim().toLowerCase()?"rgba(34,197,94,.12)":"rgba(239,68,68,.12)",color:a[i]?.trim().toLowerCase()===blanks[i].trim().toLowerCase()?"var(--green)":"var(--red)"}}>{a[i]||"___"}{a[i]?.trim().toLowerCase()!==blanks[i].trim().toLowerCase()&&<span style={{color:"var(--green)",marginLeft:5,fontSize:10}}>→ {blanks[i]}</span>}</span>:<input key={`in-${i}`} type="text" value={a[i]||""} onChange={e=>{const n=[...a];n[i]=e.target.value;setA(n)}} placeholder="…" style={{display:"inline-block",width:Math.max(70,blanks[i].length*9),padding:"2px 6px",margin:"0 2px",background:"rgba(124,92,252,.06)",border:"1px solid rgba(124,92,252,.15)",borderRadius:6,color:"var(--text)",fontSize:12,fontWeight:600,textAlign:"center",fontFamily:"Outfit"}}/>)}</span>)}</div>{!rv&&<Btn onClick={()=>setRv(true)} accent style={{width:"100%",justifyContent:"center",marginTop:12}}>Vérifier</Btn>}{rv&&<p style={{textAlign:"center",marginTop:8,fontSize:12,fontWeight:700,color:"var(--green)"}}>{blanks.filter((b,i)=>a[i]?.trim().toLowerCase()===b.trim().toLowerCase()).length}/{blanks.length} correct</p>}</div>}

function McqV({c}){const opts=useMemo(()=>{const a=[c.correct,...c.wrong];for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]]}return a},[c.id]);const[sel,setSel]=useState(null);useEffect(()=>setSel(null),[c.id]);return <div style={{...sC,marginBottom:8}}><Bdg c="var(--orange)">QCM</Bdg><RT style={{...cT,marginBottom:14}}>{c.question}</RT>{opts.map((o,i)=>{let bg="rgba(124,92,252,.04)",bd="1px solid rgba(124,92,252,.08)",cl="var(--text)";if(sel!==null){if(o===c.correct){bg="rgba(34,197,94,.1)";bd="1px solid var(--green)";cl="var(--green)"}else if(o===sel){bg="rgba(239,68,68,.1)";bd="1px solid var(--red)";cl="var(--red)"}}return <button key={i} onClick={()=>!sel&&setSel(o)} style={{display:"block",width:"100%",padding:"10px 12px",borderRadius:9,background:bg,border:bd,color:cl,textAlign:"left",cursor:sel?"default":"pointer",fontSize:12,fontWeight:500,fontFamily:"Outfit",marginBottom:6}}><span style={{opacity:.35,fontWeight:700,marginRight:6}}>{String.fromCharCode(65+i)}</span>{o}</button>})}{sel&&<p style={{textAlign:"center",fontSize:12,fontWeight:700,color:sel===c.correct?"var(--green)":"var(--red)"}}>{sel===c.correct?"✓ Correct":"✗ Raté"}</p>}</div>}

function OpenV({c}){const[sh,setSh]=useState(false);useEffect(()=>setSh(false),[c.id]);return <div style={{...sC,marginBottom:8}}><Bdg c="var(--accent2)">Question ouverte</Bdg><RT style={cT}>{c.question}</RT>{!sh?<Btn onClick={()=>setSh(true)} accent style={{width:"100%",justifyContent:"center",marginTop:14}}>Voir la réponse</Btn>:<div style={{marginTop:14,padding:12,borderRadius:9,background:"rgba(34,197,94,.05)",borderLeft:"3px solid var(--green)"}}><div style={{fontSize:9,fontWeight:700,color:"var(--green)",textTransform:"uppercase",letterSpacing:1,marginBottom:5}}>Réponse attendue</div><RT style={{color:"var(--text)",fontSize:12,lineHeight:1.6}}>{c.answer}</RT></div>}</div>}

/* ═══════ EXPORT ═══════ */
function ExportSection({cards,exportDeck}){
  const[open,setOpen]=useState(false);
  const tree=useMemo(()=>buildTree(cards),[cards]);
  if(!cards.length) return null;
  return <div style={{marginTop:4}}>
    <Btn onClick={()=>setOpen(!open)} small ghost style={{width:"100%",justifyContent:"center"}}><I.Export/> Exporter{open?" ▾":" ▸"}</Btn>
    {open&& <div style={{...P,marginTop:8,padding:12}}>
      <div style={{fontSize:10,color:"var(--text3)",marginBottom:8}}>JSON prêt à réimporter ou partager.</div>
      <Btn onClick={()=>exportDeck(null)} small ghost style={{width:"100%",justifyContent:"center",marginBottom:8}}>Tout ({cards.length} cartes)</Btn>
      {tree.map(n=> <ExportNode key={n.path} n={n} dep={0} exportDeck={exportDeck}/>)}
    </div>}
  </div>;
}
function ExportNode({n,dep,exportDeck}){
  const[open,setOpen]=useState(false);
  return <div style={{marginLeft:dep*12,marginBottom:4}}>
    <div style={{display:"flex",alignItems:"center",gap:5}}>
      {n.children.length>0&&<span onClick={()=>setOpen(!open)} style={{cursor:"pointer",lineHeight:0}}><I.Chev open={open}/></span>}
      <span style={{fontSize:12,fontWeight:500,color:"var(--text2)",flex:1,overflow:"hidden",textOverflow:"ellipsis",whiteSpace:"nowrap"}}>{n.name}</span>
      <span style={{fontSize:10,color:"var(--text3)",flexShrink:0,marginRight:4}}>{n.total}c</span>
      <Btn onClick={()=>exportDeck(n.path)} small ghost style={{padding:"3px 8px"}}><I.Export/></Btn>
    </div>
    {open&&n.children.map(ch=> <ExportNode key={ch.path} n={ch} dep={dep+1} exportDeck={exportDeck}/>)}
  </div>;
}

/* ═══════ SHARED ═══════ */
function Btn({children,onClick,accent,danger,small,ghost,style,disabled}){
  return <button onClick={onClick} disabled={disabled} style={{display:"flex",alignItems:"center",gap:4,padding:small?"5px 10px":"9px 16px",borderRadius:9,background:accent?"var(--accent)":danger?"var(--red)":ghost?"transparent":"var(--bg3)",border:ghost?"1px solid rgba(124,92,252,.12)":"none",color:accent||danger?"#fff":"var(--text2)",fontSize:small?11:13,fontWeight:600,cursor:disabled?"default":"pointer",fontFamily:"Outfit",transition:"all .15s",opacity:disabled?0.4:1,...style}}>{children}</button>;
}
function Bdg({children,c}){return <span style={{display:"inline-block",padding:"2px 8px",borderRadius:6,fontSize:9,fontWeight:700,background:`color-mix(in srgb,${c} 12%,transparent)`,color:c,textTransform:"uppercase",letterSpacing:.7}}>{children}</span>}
function Inp({label,value,onChange,ph,mini,multi}){const T=multi?"textarea":"input";return <div style={{marginBottom:mini?6:12}}>{label&&<label style={{fontSize:11,fontWeight:600,color:"var(--text2)",display:"block",marginBottom:3}}>{label}</label>}<T value={value} onChange={e=>onChange(e.target.value)} placeholder={ph} rows={multi?3:undefined} style={{width:"100%",background:"var(--bg3)",border:"1px solid rgba(124,92,252,.1)",borderRadius:8,color:"var(--text)",padding:"8px 10px",fontSize:12,fontFamily:"Outfit",resize:multi?"vertical":"none"}}/></div>}
function PT({children}){return <div style={{fontSize:11,fontWeight:700,color:"var(--text3)",marginBottom:10,textTransform:"uppercase",letterSpacing:.8}}>{children}</div>}
function Empty({e,t}){return <div style={{...P,textAlign:"center",padding:"36px 16px"}}><div style={{fontSize:36,marginBottom:8}}>{e}</div><p style={{color:"var(--text3)",fontSize:12}}>{t}</p></div>}
const P={background:"var(--bg2)",borderRadius:12,padding:14,border:"1px solid rgba(124,92,252,.05)",marginBottom:10};
const fS={width:"100%",minHeight:140,background:"var(--bg2)",borderRadius:14,padding:"20px 16px",boxSizing:"border-box",display:"flex",flexDirection:"column",justifyContent:"center",wordBreak:"break-word",overflowWrap:"break-word"};
const sC={background:"var(--bg2)",borderRadius:14,padding:"18px 16px",border:"1px solid rgba(124,92,252,.06)",minHeight:120,wordBreak:"break-word",overflowWrap:"break-word"};
const cT={fontSize:14,lineHeight:1.7,color:"var(--text)",marginTop:10,fontWeight:500,wordBreak:"break-word",overflowWrap:"break-word"};
const taS={width:"100%",background:"var(--bg2)",border:"1px solid rgba(124,92,252,.1)",borderRadius:10,color:"var(--text)",padding:12,fontSize:12,fontFamily:"JetBrains Mono,monospace",resize:"vertical",lineHeight:1.5};
const selS={width:"100%",background:"var(--bg3)",border:"1px solid rgba(124,92,252,.12)",borderRadius:7,color:"var(--text)",padding:"4px 8px",fontSize:11,fontFamily:"Outfit",cursor:"pointer"};

ReactDOM.createRoot(document.getElementById("root")).render(React.createElement(App));
</script>
</body>
</html>
