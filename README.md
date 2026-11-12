<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>دِرع ثلاثي الأبعاد</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Arabic:wght@400;600;700&family=JetBrains+Mono:wght@400;600&display=swap">
<style>
/* Layout: 3D stage, then a live "raw vs calibrated" readout driven by a sample-volume slider */
:root{
  --bg:#f3f6f7; --surface:#ffffff; --ink:#10242b; --muted:#5a6f76; --line:#d5e0e3;
  --teal:#0f7a7a; --teal-soft:#d9efee; --hit:#c23b4e; --hit-soft:#f8dde1; --cal:#3b6fb6; --cal-soft:#dce7f6;
  --stage:#e6eef0;
  --f-display:"IBM Plex Sans Arabic",Tahoma,system-ui,sans-serif;
  --f-body:"IBM Plex Sans Arabic",Tahoma,system-ui,sans-serif;
  --f-mono:"JetBrains Mono",ui-monospace,monospace;
  color-scheme:light;
}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){
  --bg:#0d1a1e; --surface:#132429; --ink:#e4eef0; --muted:#93a9ae; --line:#24393f;
  --teal:#45b8b2; --teal-soft:#16393a; --hit:#ef6b7c; --hit-soft:#3d1d24; --cal:#7aa5e6; --cal-soft:#1c2c45; --stage:#10272c; color-scheme:dark}}
:root[data-theme="dark"]{
  --bg:#0d1a1e; --surface:#132429; --ink:#e4eef0; --muted:#93a9ae; --line:#24393f;
  --teal:#45b8b2; --teal-soft:#16393a; --hit:#ef6b7c; --hit-soft:#3d1d24; --cal:#7aa5e6; --cal-soft:#1c2c45; --stage:#10272c; color-scheme:dark}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font-family:var(--f-body);font-size:15px;line-height:1.55}
.wrap{max-width:1000px;margin:0 auto;padding:24px 16px 44px;display:grid;gap:18px}
.eyebrow{font-size:13px;font-weight:600;text-transform:uppercase;color:var(--teal)}
h1{font-family:var(--f-display);font-size:clamp(26px,5vw,38px);line-height:1.1;margin:4px 0 6px;text-wrap:balance}
h2{font-family:var(--f-display);font-size:19px;margin:0}
.lede{margin:0;color:var(--muted);max-width:66ch}
.stage{position:relative;background:var(--stage);border:1px solid var(--line);border-radius:16px;overflow:hidden;aspect-ratio:16/9;max-width:100%}
@media (max-width:600px){.stage{aspect-ratio:1/1}}
canvas{display:block;width:100%;height:100%;touch-action:none}
.hint{position:absolute;right:12px;bottom:10px;font-family:var(--f-mono);font-size:11px;color:var(--muted)}
.btns{display:flex;gap:8px;flex-wrap:wrap}
button{font:inherit;font-weight:600;padding:9px 16px;border-radius:999px;border:1px solid var(--teal);background:transparent;color:var(--teal);cursor:pointer}
button[aria-pressed="true"]{background:var(--teal);color:var(--surface)}
button:focus-visible,input:focus-visible{outline:3px solid var(--cal);outline-offset:2px}
.panel{background:var(--surface);border:1px solid var(--line);border-radius:14px;padding:18px;display:grid;gap:14px;min-width:0}
.ctrl{display:grid;gap:6px}
.ctrl label{font-weight:600;font-size:14px}
input[type=range]{width:100%;accent-color:var(--teal)}
.scale{display:flex;gap:8px;justify-content:space-between;font-family:var(--f-mono);font-size:11px;color:var(--muted)}
.cols{display:grid;grid-template-columns:1fr 1fr;gap:14px}
@media (max-width:700px){.cols{grid-template-columns:1fr}}
.col{border:1px solid var(--line);border-radius:12px;padding:14px;display:grid;gap:10px;min-width:0}
.col h3{margin:0;font-size:15px;color:var(--muted)}
.row{display:grid;grid-template-columns:84px 1fr 46px;align-items:center;gap:8px;font-size:13px}
.row .nm{font-family:var(--f-mono);font-size:12px;direction:ltr;text-align:right}
.bar{direction:ltr;position:relative;height:14px;background:var(--bg);border-radius:7px;overflow:hidden}
.fill{position:absolute;inset:0 auto 0 0;background:var(--teal);border-radius:7px;transition:width .25s}
.fill.pos{background:var(--hit)}
.cut{position:absolute;top:-3px;bottom:-3px;width:2px;background:var(--ink);opacity:.55}
.val{font-family:var(--f-mono);font-size:12px;text-align:left;direction:ltr;font-variant-numeric:tabular-nums}
.verdict{border-radius:10px;padding:10px 12px;font-size:14px;font-weight:600}
.v-pos{background:var(--hit-soft);color:var(--ink)}
.v-neg{background:var(--teal-soft);color:var(--ink)}
.v-bad{background:var(--cal-soft);color:var(--ink)}
.legend{display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:10px}
.layer{background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:12px 14px;display:grid;gap:2px;min-width:0}
.layer i{font-style:normal;font-size:12px;font-weight:600;color:var(--teal)}
.layer b{font-size:14px}.layer span{font-size:13px;color:var(--muted)}
.note{font-size:12.5px;color:var(--muted);margin:0}
@media (prefers-reduced-motion:reduce){*{transition:none!important}}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <span class="eyebrow">نموذج تصوري · بيانات محاكاة · ليس بالمقاس الحقيقي</span>
    <h1>دِرع: لاصقة ذاتية المعايرة لفرز خطر سرطان الرئة</h1>
    <p class="lede">خمس مناطق من الإبر تصطاد الأجسام المضادة الذاتية ضد بروتينات أورام الرئة، ومنطقة سادسة تقيس الأجسام المضادة الكلية IgG من العينة نفسها. لذلك تُقرأ كل نتيجة كنسبة، وتبقى موثوقة حتى لو سحبت الإبر سائلًا قليلًا.</p>
  </header>

  <div class="stage" id="stage">
    <canvas id="c" aria-label="نموذج ثلاثي الأبعاد للاصقة دِرع"></canvas>
    <span class="hint">اسحب للتدوير · قرّب بإصبعين أو بعجلة الفأرة</span>
  </div>

  <div class="btns">
    <button id="bExplode" type="button" aria-pressed="false">تفكيك الطبقات</button>
    <button id="bCase" type="button" aria-pressed="true">الحالة: توجد إشارة سرطان</button>
    <button id="bSkin" type="button" aria-pressed="true">الجلد</button>
    <button id="bSpin" type="button" aria-pressed="true">دوران تلقائي</button>
  </div>

  <section class="panel">
    <h2>لماذا نحتاج المعايرة؟</h2>
    <div class="ctrl">
      <label for="vol">كمية السائل التي سحبتها الإبر: <span id="volTxt"></span></label>
      <input id="vol" dir="ltr" type="range" min="0.1" max="1.5" step="0.05" value="0.35">
      <div class="scale" dir="ltr"><span>قليلة جدًا (جلد رقيق وجاف)</span><span>معتادة</span><span>كثيرة</span></div>
    </div>
    <div class="cols">
      <div class="col">
        <h3>شدة اللون وحدها</h3>
        <div id="raw"></div>
        <div class="verdict" id="rawV"></div>
      </div>
      <div class="col">
        <h3>دِرع: النسبة إلى IgG الكلي</h3>
        <div id="ratio"></div>
        <div class="verdict" id="ratioV"></div>
      </div>
    </div>
    <p class="note">حرّك الشريط إلى اليسار. عندما يكون السائل قليلًا يبهت اللون وينزل تحت الحد، فتضيع إشارة حقيقية. أما النسبة فتبقى ثابتة لأن منطقة المعايرة تبهت بالقدر نفسه. وإذا نزلت منطقة المعايرة تحت حد أدنى، ترفض اللاصقة العينة بدل أن تعطي نتيجة سلبية كاذبة.</p>
  </section>

  <div class="legend">
    <div class="layer"><i>الطبقة ١</i><b>الفيلم الخارجي</b><span>نافذة شفافة لقراءة المناطق</span></div>
    <div class="layer"><i>الطبقة ٢</i><b>الحلقة اللاصقة</b><span>تثبّت اللاصقة على الساعد طوال مدة الاستخدام</span></div>
    <div class="layer"><i>الطبقة ٣</i><b>طبقة القراءة</b><span>جسم مضاد مُعلَّم يلوّن كل منطقة، وتطبيق الهاتف يقرأ شدة اللون</span></div>
    <div class="layer"><i>الطبقة ٤</i><b>مصفوفة الإبر المجهرية</b><span>خمس مناطق لمستضدات الرئة (p53 وNY-ESO-1 وSOX2 وCAGE وMAGE-A4) ومنطقة معايرة للـIgG الكلي</span></div>
  </div>
  <p class="note">تصوّر مبدئي لعرض الهاكاثون. كل القيم أمثلة محاكاة وليست بيانات مقاسة. دِرع أداة فرز للخطر وليست تشخيصًا.</p>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
<script>
(function(){
const ZONES=[
 {id:"p53",name:"p53",pos:[-1.7,-0.9]},
 {id:"nyeso",name:"NY-ESO-1",pos:[0,-0.9]},
 {id:"sox2",name:"SOX2",pos:[1.7,-0.9]},
 {id:"cage",name:"CAGE",pos:[-1.7,0.9]},
 {id:"mage",name:"MAGE-A4",pos:[0,0.9]},
 {id:"cal",name:"IgG",pos:[1.7,0.9],cal:true}
];
const BASE=0.06, POS=0.32, RAW_CUT=0.15, RATIO_CUT=0.15, CAL_MIN=0.2;
const hits=["nyeso","sox2"];
let positive=true;

/* ---------- readout ---------- */
const vol=document.getElementById("vol");
function level(z,v){ if(z.cal) return v; return v*((positive&&hits.includes(z.id))?POS:BASE); }
function rowHTML(name,value,max,cut,pos){
  const w=Math.min(100,value/max*100), c=Math.min(100,cut/max*100);
  return `<div class="row"><span class="nm">${name}</span><div class="bar"><div class="fill${pos?" pos":""}" style="width:${w}%"></div><div class="cut" style="left:${c}%"></div></div><span class="val">${value.toFixed(2)}</span></div>`;
}
function update(){
  const v=+vol.value;
  document.getElementById("volTxt").textContent=(v*20).toFixed(0)+" ميكرولتر (محاكاة)";
  const cal=v, ok=cal>=CAL_MIN;
  let raw="",rat="",rawPos=0,ratPos=0;
  ZONES.filter(z=>!z.cal).forEach(z=>{
    const s=level(z,v), r=s/cal;
    const rp=s>=RAW_CUT, qp=r>=RATIO_CUT;
    if(rp)rawPos++; if(qp)ratPos++;
    raw+=rowHTML(z.name,s,0.5,RAW_CUT,rp);
    rat+=rowHTML(z.name,r,0.5,RATIO_CUT,qp&&ok);
  });
  document.getElementById("raw").innerHTML=raw;
  document.getElementById("ratio").innerHTML=rat+`<div class="row"><span class="nm">IgG</span><div class="bar"><div class="fill" style="width:${Math.min(100,cal/1.5*100)}%;background:var(--cal)"></div><div class="cut" style="left:${CAL_MIN/1.5*100}%"></div></div><span class="val">${cal.toFixed(2)}</span></div>`;
  const rv=document.getElementById("rawV"), qv=document.getElementById("ratioV");
  if(rawPos){rv.className="verdict v-pos";rv.textContent=`إشارة في ${rawPos} ${rawPos>1?"منطقتين":"منطقة"}: إحالة`;}
  else{rv.className="verdict v-neg";rv.textContent=positive?"تُقرأ سلبية: ضاعت الإشارة (سلبية كاذبة)":"تُقرأ سلبية";}
  if(!ok){qv.className="verdict v-bad";qv.textContent="المعايرة ضعيفة: تُرفض العينة ويُعاد الفحص";}
  else if(ratPos){qv.className="verdict v-pos";qv.textContent=`إشارة في ${ratPos>1?"منطقتين":"منطقة"}: إحالة لفحص دم وأشعة مقطعية`;}
  else{qv.className="verdict v-neg";qv.textContent="لا توجد إشارة: استمرار المتابعة الدورية";}
  paint(v,ok);
}

/* ---------- 3D ---------- */
const canvas=document.getElementById("c"),stage=document.getElementById("stage");
const renderer=new THREE.WebGLRenderer({canvas,antialias:true,alpha:true});
renderer.setPixelRatio(Math.min(devicePixelRatio,2));
const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(35,1,0.1,200);camera.position.set(9,8,11);
const controls=new THREE.OrbitControls(camera,canvas);
controls.enableDamping=true;controls.target.set(0,0.6,0);controls.minDistance=6;controls.maxDistance=30;
const reduce=matchMedia("(prefers-reduced-motion: reduce)").matches;
controls.autoRotate=!reduce;controls.autoRotateSpeed=1.1;
document.getElementById("bSpin").setAttribute("aria-pressed",String(controls.autoRotate));
scene.add(new THREE.HemisphereLight(0xffffff,0x8aa0a6,0.9));
const key=new THREE.DirectionalLight(0xffffff,0.8);key.position.set(6,12,8);scene.add(key);

const skin=new THREE.Mesh(new THREE.BoxGeometry(9.5,0.8,9),new THREE.MeshStandardMaterial({color:0xe9c4ab,roughness:0.9,transparent:true,opacity:0.55}));
skin.position.y=-0.4;scene.add(skin);

function roundedShape(w,d,r){const s=new THREE.Shape(),x=-w/2,y=-d/2;
 s.moveTo(x+r,y);s.lineTo(x+w-r,y);s.quadraticCurveTo(x+w,y,x+w,y+r);s.lineTo(x+w,y+d-r);
 s.quadraticCurveTo(x+w,y+d,x+w-r,y+d);s.lineTo(x+r,y+d);s.quadraticCurveTo(x,y+d,x,y+d-r);
 s.lineTo(x,y+r);s.quadraticCurveTo(x,y,x+r,y);return s;}
function slab(shape,h,mat){const g=new THREE.ExtrudeGeometry(shape,{depth:h,bevelEnabled:false});g.rotateX(-Math.PI/2);return new THREE.Mesh(g,mat);}
const W=5.6,D=4;

const L4=new THREE.Group();
L4.add(slab(roundedShape(W,D,0.5),0.18,new THREE.MeshStandardMaterial({color:0xdfe8ea,roughness:0.5})));
const needleMat=new THREE.MeshStandardMaterial({color:0xc9d6da,metalness:0.2,roughness:0.35});
const calNeedle=new THREE.MeshStandardMaterial({color:0x9fb6d8,metalness:0.2,roughness:0.35});
const cone=new THREE.ConeGeometry(0.09,0.55,12);cone.rotateX(Math.PI);cone.translate(0,-0.275,0);
ZONES.forEach(z=>{const[x,y]=z.pos;for(let i=-1;i<=1;i++)for(let k=-1;k<=1;k++){
  const n=new THREE.Mesh(cone,z.cal?calNeedle:needleMat);n.position.set(x+i*0.32,0,y+k*0.32);L4.add(n);}});
scene.add(L4);

const L3=new THREE.Group();
L3.add(slab(roundedShape(W,D,0.5),0.06,new THREE.MeshStandardMaterial({color:0xf6fafb,roughness:0.6})));
const zoneMesh={};
const cA=new THREE.Color(0xdcefee), cHit=new THREE.Color(0xc23b4e), cCal=new THREE.Color(0x3b6fb6), cCalA=new THREE.Color(0xdce7f6), cBase=new THREE.Color(0x8fc9c4);
ZONES.forEach(z=>{const[x,y]=z.pos;
  const m=new THREE.Mesh(new THREE.CylinderGeometry(0.58,0.58,0.05,40),new THREE.MeshStandardMaterial({color:0xdcefee,roughness:0.5}));
  m.position.set(x,0.085,y);L3.add(m);zoneMesh[z.id]=m;
  if(z.cal){const ring=new THREE.Mesh(new THREE.TorusGeometry(0.66,0.04,8,48),new THREE.MeshStandardMaterial({color:0x3b6fb6}));ring.rotation.x=Math.PI/2;ring.position.set(x,0.09,y);L3.add(ring);}
});
scene.add(L3);

const L2=new THREE.Group();
const ring=roundedShape(W+0.8,D+0.8,0.8);
const hole=new THREE.Path();(function(){const hw=W-0.2,hd=D-0.2,hx=-hw/2,hy=-hd/2;hole.moveTo(hx,hy);hole.lineTo(hx+hw,hy);hole.lineTo(hx+hw,hy+hd);hole.lineTo(hx,hy+hd);hole.lineTo(hx,hy);})();
ring.holes.push(hole);
L2.add(slab(ring,0.05,new THREE.MeshStandardMaterial({color:0xe7d9b8,roughness:0.8})));
scene.add(L2);

const L1=new THREE.Group();
L1.add(slab(roundedShape(W+0.8,D+0.8,0.8),0.06,new THREE.MeshStandardMaterial({color:0xeaf3f5,transparent:true,opacity:0.42,roughness:0.2})));
scene.add(L1);

function paint(v,ok){
  ZONES.forEach(z=>{
    const m=zoneMesh[z.id].material;
    if(z.cal){m.color.copy(cCalA).lerp(cCal,Math.min(1,v/1.2));return;}
    const s=level(z,v);
    const target=(positive&&hits.includes(z.id))?cHit:cBase;
    m.color.copy(cA).lerp(target,Math.min(1,s/0.3));
  });
}

const base={L4:0.02,L3:0.2,L2:0.02,L1:0.34},spread={L4:0,L3:1.3,L2:2.4,L1:3.4};
let exploded=false,t=0;
function place(){L4.position.y=base.L4+spread.L4*t;L3.position.y=base.L3+spread.L3*t;L2.position.y=base.L2+spread.L2*t;L1.position.y=base.L1+spread.L1*t;}

function toggle(id,fn){const b=document.getElementById(id);b.addEventListener("click",()=>{const v=b.getAttribute("aria-pressed")!=="true";b.setAttribute("aria-pressed",String(v));fn(v,b);});}
toggle("bExplode",v=>exploded=v);
toggle("bCase",(v,b)=>{positive=v;b.textContent=v?"الحالة: توجد إشارة سرطان":"الحالة: لا توجد إشارة";update();});
toggle("bSkin",v=>skin.visible=v);
toggle("bSpin",v=>controls.autoRotate=v);
vol.addEventListener("input",update);

function resize(){const r=stage.getBoundingClientRect();renderer.setSize(r.width,r.height,false);camera.aspect=r.width/r.height;camera.updateProjectionMatrix();}
new ResizeObserver(resize).observe(stage);resize();
function loop(){const target=exploded?1:0;t+=reduce?(target-t):(target-t)*0.08;place();controls.update();renderer.render(scene,camera);requestAnimationFrame(loop);}
update();loop();
})();
</script>
</body>
</html>
