import { useState, useEffect } from "react";

const C = {
  red: "#D4172B", redDark: "#A8111F", redLight: "#E8253A",
  white: "#FFFFFF", cream: "#FFF7F7",
  gray: "#9B6B70", grayLight: "#FCEAEC", grayBorder: "#F5DEDE",
  text: "#2A0A0D", textSoft: "#6B3040",
};

const CATS = [
  { id: "all", label: "الكل" }, { id: "tech", label: "تكنولوجيا" },
  { id: "marketing", label: "تسويق" }, { id: "finance", label: "مالية" },
  { id: "engineering", label: "هندسة" }, { id: "design", label: "تصميم" },
  { id: "sales", label: "مبيعات" }, { id: "hr", label: "موارد بشرية" },
];

const WILAYAS = ["الجزائر العاصمة","وهران","قسنطينة","عنابة","سطيف","باتنة","تلمسان","بجاية","بسكرة","ورقلة"];

const JOBS = [
  { id:1, title:"مطور واجهات أمامية", company:"تك فيجن الجزائر", logo:"T", location:"الجزائر العاصمة", type:"دوام كامل", category:"tech", salary:"80,000 - 120,000 دج", posted:"منذ يومين", urgent:true, remote:false, tags:["React","JavaScript","CSS"], desc:"نبحث عن مطور واجهات أمامية متمرس للانضمام إلى فريقنا التقني.", requirements:["خبرة 2+ سنوات في React","إتقان JavaScript وCSS","معرفة بـ Git"], benefits:["تأمين صحي","تدريب مستمر","بيئة عمل مرنة"] },
  { id:2, title:"مختص شراء إعلانات", company:"دلتا ميديا", logo:"D", location:"وهران", type:"دوام كامل", category:"marketing", salary:"60,000 - 90,000 دج", posted:"منذ 3 أيام", urgent:false, remote:true, tags:["Meta Ads","Google Ads","TikTok"], desc:"فرصة للانضمام إلى وكالة إعلانية رائدة كمختص في إدارة الحملات الإعلانية الرقمية.", requirements:["خبرة في Meta وGoogle Ads","فهم مؤشرات الأداء","خبرة 1+ سنة"], benefits:["عمولات على النتائج","عمل عن بُعد جزئي","دورات تدريبية"] },
  { id:3, title:"محاسب أول", company:"مجموعة الأطلس", logo:"أ", location:"قسنطينة", type:"دوام كامل", category:"finance", salary:"70,000 - 100,000 دج", posted:"منذ أسبوع", urgent:false, remote:false, tags:["محاسبة","Excel","IFRS"], desc:"نحن بحاجة إلى محاسب أول ذو خبرة لمتابعة الملفات المحاسبية والضريبية.", requirements:["شهادة محاسبة أو مالية","خبرة 3+ سنوات","إتقان برامج المحاسبة"], benefits:["راتب تنافسي","تأمين صحي","مكافآت سنوية"] },
  { id:4, title:"مصمم جرافيك", company:"إبداع ستوديو", logo:"إ", location:"الجزائر العاصمة", type:"عقد حر", category:"design", salary:"حسب المشروع", posted:"منذ يوم", urgent:true, remote:true, tags:["Illustrator","Photoshop","Canva"], desc:"مطلوب مصمم جرافيك موهوب للعمل على مشاريع متنوعة.", requirements:["إتقان Adobe Suite","حس إبداعي عالي","portfolio قوي"], benefits:["مرونة في العمل","مشاريع متنوعة","شبكة علاقات"] },
  { id:5, title:"مهندس مدني", company:"البناء الذهبي", logo:"ب", location:"عنابة", type:"دوام كامل", category:"engineering", salary:"90,000 - 140,000 دج", posted:"منذ 5 أيام", urgent:false, remote:false, tags:["AutoCAD","BIM","إدارة مشاريع"], desc:"فرصة للانضمام إلى فريق هندسي متخصص في مشاريع البنية التحتية.", requirements:["شهادة هندسة مدنية","خبرة 2+ سنوات","إتقان AutoCAD"], benefits:["سكن إداري","تنقل مدفوع","مكافآت إنجاز"] },
  { id:6, title:"مدير تسويق رقمي", company:"سمارت كوميرس", logo:"س", location:"الجزائر العاصمة", type:"دوام كامل", category:"marketing", salary:"100,000 - 150,000 دج", posted:"منذ أسبوع", urgent:false, remote:true, tags:["SEO","Content","Analytics"], desc:"نبحث عن مدير تسويق رقمي استراتيجي لقيادة فريق وبناء حضور رقمي قوي.", requirements:["5+ سنوات تسويق رقمي","خبرة قيادة فرق","تفكير استراتيجي"], benefits:["سيارة الشركة","حصة في الأرباح","عمل هجين"] },
  { id:7, title:"مختص موارد بشرية", company:"فنادق النجوم", logo:"ن", location:"تيزي وزو", type:"دوام كامل", category:"hr", salary:"55,000 - 75,000 دج", posted:"منذ 4 أيام", urgent:false, remote:false, tags:["توظيف","تدريب","قانون العمل"], desc:"مطلوب أخصائي موارد بشرية لإدارة دورة حياة الموظف كاملة.", requirements:["شهادة في الإدارة","معرفة قانون العمل","مهارات تواصل"], benefits:["إقامة مجانية","وجبات مدعومة","تكوين مستمر"] },
  { id:8, title:"مندوب مبيعات", company:"اكسبريس للتوزيع", logo:"ا", location:"سطيف", type:"دوام كامل", category:"sales", salary:"45,000 + عمولات", posted:"منذ يومين", urgent:true, remote:false, tags:["مبيعات","CRM","تفاوض"], desc:"انضم إلى فريق مبيعاتنا النشط وساهم في توسيع شبكة عملائنا.", requirements:["خبرة مبيعات ميدانية","رخصة قيادة","شخصية مقنعة"], benefits:["سيارة الشركة","هاتف ووقود","عمولات مجزية"] },
];

export default function Tawjih() {
  const [tab, setTab] = useState("home");
  const [cat, setCat] = useState("all");
  const [search, setSearch] = useState("");
  const [wilaya, setWilaya] = useState("");
  const [job, setJob] = useState(null);
  const [saved, setSaved] = useState([1,4]);
  const [loaded, setLoaded] = useState(false);

  useEffect(() => { setTimeout(() => setLoaded(true), 150); }, []);

  const jobs = JOBS.filter(j =>
    (cat === "all" || j.category === cat) &&
    (!search || j.title.includes(search) || j.company.includes(search) || j.tags.some(t => t.toLowerCase().includes(search.toLowerCase()))) &&
    (!wilaya || j.location === wilaya)
  );

  const toggleSave = (id) => setSaved(p => p.includes(id) ? p.filter(x => x !== id) : [...p, id]);

  return (
    <div style={{ direction:"rtl", fontFamily:"'Cairo',sans-serif", background:C.cream, minHeight:"100vh", color:C.text }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@600;700;800;900&display=swap');
        *{box-sizing:border-box;margin:0;padding:0}
        button{font-family:'Cairo',sans-serif}
        input,select{font-family:'Cairo',sans-serif}
        ::-webkit-scrollbar{width:5px;height:5px}
        ::-webkit-scrollbar-thumb{background:${C.red};border-radius:3px}
        .jcard:hover{transform:translateY(-4px)!important;box-shadow:0 16px 40px rgba(212,23,43,0.14)!important;border-color:${C.red}!important}
        .catbtn:hover{background:${C.redLight}!important;color:#fff!important;border-color:${C.redLight}!important}
        .navbtn:hover{background:rgba(255,255,255,0.15)!important}
      `}</style>

      {/* ═══ NAV ═══ */}
      <nav style={{ background:`linear-gradient(135deg,${C.redDark},${C.red})`, height:60, display:"flex", alignItems:"center", justifyContent:"space-between", padding:"0 24px", position:"sticky", top:0, zIndex:100, boxShadow:"0 4px 20px rgba(168,17,31,0.4)" }}>
        <div style={{ display:"flex", alignItems:"center", gap:10 }}>
          <div style={{ width:36, height:36, background:"rgba(255,255,255,0.2)", borderRadius:10, border:"2px solid rgba(255,255,255,0.35)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:16, fontWeight:900, color:"#fff" }}>ت</div>
          <div>
            <div style={{ color:"#fff", fontSize:20, fontWeight:900, lineHeight:1 }}>توجيه</div>
            <div style={{ color:"rgba(255,255,255,0.6)", fontSize:9, fontWeight:700, letterSpacing:2 }}>TAWJIH · DZ</div>
          </div>
        </div>
        <div style={{ display:"flex", gap:6, alignItems:"center" }}>
          {[["home","الرئيسية"],["saved","المحفوظات"],["about","من نحن"]].map(([id,lbl]) => (
            <button key={id} className="navbtn" onClick={() => setTab(id)} style={{ background: tab===id ? "rgba(255,255,255,0.2)" : "transparent", color:"#fff", border: tab===id ? "1px solid rgba(255,255,255,0.35)" : "1px solid transparent", borderRadius:8, padding:"7px 14px", cursor:"pointer", fontSize:13, fontWeight:700, transition:"all .2s" }}>{lbl}</button>
          ))}
          <button style={{ background:"#fff", color:C.red, border:"none", borderRadius:8, padding:"8px 16px", fontWeight:900, fontSize:13, cursor:"pointer", boxShadow:"0 2px 10px rgba(0,0,0,0.15)" }}>+ انشر وظيفة</button>
        </div>
      </nav>

      {/* ═══ HERO ═══ */}
      {tab === "home" && (
        <div style={{ background:`linear-gradient(145deg,${C.redDark} 0%,${C.red} 55%,${C.redLight} 100%)`, padding:"60px 24px 48px", textAlign:"center", position:"relative", overflow:"hidden" }}>
          <div style={{ position:"absolute", inset:0, backgroundImage:`radial-gradient(circle,rgba(255,255,255,0.05) 1px,transparent 1px)`, backgroundSize:"28px 28px", pointerEvents:"none" }} />
          <div style={{ position:"absolute", inset:0, background:`radial-gradient(circle at 20% 60%,rgba(255,255,255,0.08) 0%,transparent 50%)`, pointerEvents:"none" }} />
          <div style={{ position:"relative", zIndex:1, opacity: loaded?1:0, transform: loaded?"translateY(0)":"translateY(24px)", transition:"all .7s ease" }}>
            <div style={{ display:"inline-flex", alignItems:"center", gap:8, background:"rgba(255,255,255,0.15)", border:"1px solid rgba(255,255,255,0.3)", color:"#fff", padding:"6px 18px", borderRadius:100, fontSize:12, fontWeight:700, marginBottom:20 }}>
              🇩🇿 المنصة الجزائرية الأولى للتوظيف
            </div>
            <h1 style={{ fontSize:"clamp(1.8rem,4.5vw,3rem)", fontWeight:900, color:"#fff", lineHeight:1.25, marginBottom:12 }}>
              ابحث عن{" "}
              <span style={{ background:"linear-gradient(135deg,#FFD6DA,#fff)", WebkitBackgroundClip:"text", WebkitTextFillColor:"transparent" }}>وظيفة أحلامك</span>
              <br />في الجزائر
            </h1>
            <p style={{ color:"rgba(255,255,255,0.75)", fontSize:16, marginBottom:32 }}>آلاف الفرص · 48 ولاية · توظيف حقيقي</p>

            {/* Search */}
            <div style={{ display:"flex", gap:10, maxWidth:680, margin:"0 auto 36px", background:"#fff", borderRadius:14, padding:8, boxShadow:"0 20px 60px rgba(0,0,0,0.22)" }}>
              <input value={search} onChange={e=>setSearch(e.target.value)} placeholder="ابحث عن وظيفة أو شركة..." style={{ flex:1, border:"none", outline:"none", padding:"10px 14px", fontSize:14, background:"transparent", direction:"rtl", color:C.text }} />
              <select value={wilaya} onChange={e=>setWilaya(e.target.value)} style={{ border:`1px solid ${C.grayBorder}`, borderRadius:8, padding:"8px 12px", fontSize:13, background:C.cream, outline:"none", direction:"rtl", color:C.text, cursor:"pointer" }}>
                <option value="">كل الولايات</option>
                {WILAYAS.map(w => <option key={w} value={w}>{w}</option>)}
              </select>
              <button style={{ background:`linear-gradient(135deg,${C.redDark},${C.red})`, color:"#fff", border:"none", borderRadius:10, padding:"10px 24px", fontSize:14, fontWeight:800, cursor:"pointer" }}>🔍 بحث</button>
            </div>

            {/* Stats */}
            <div style={{ display:"flex", justifyContent:"center", gap:32, flexWrap:"wrap" }}>
              {[["2,400+","وظيفة نشطة"],["850+","شركة موثوقة"],["18,000+","مسجّل"],["48 ولاية","تغطية وطنية"]].map(([v,l]) => (
                <div key={l} style={{ textAlign:"center" }}>
                  <div style={{ fontSize:26, fontWeight:900, color:"#fff" }}>{v}</div>
                  <div style={{ fontSize:11, color:"rgba(255,255,255,0.6)", fontWeight:700 }}>{l}</div>
                </div>
              ))}
            </div>
          </div>
        </div>
      )}

      {/* ═══ HOME CONTENT ═══ */}
      {tab === "home" && (
        <div style={{ maxWidth:1200, margin:"0 auto", padding:"28px 20px" }}>

          {/* Categories */}
          <div style={{ marginBottom:8 }}>
            <div style={{ fontSize:20, fontWeight:800, color:C.text, marginBottom:4 }}>تصفح حسب المجال</div>
            <div style={{ width:36, height:3, background:C.red, borderRadius:2, marginBottom:16 }} />
            <div style={{ display:"flex", gap:10, overflowX:"auto", paddingBottom:10, scrollbarWidth:"none" }}>
              {CATS.map(c => (
                <button key={c.id} className="catbtn" onClick={() => setCat(c.id)} style={{ background: cat===c.id ? C.red : "#fff", color: cat===c.id ? "#fff" : C.text, border:`2px solid ${cat===c.id ? C.red : C.grayBorder}`, borderRadius:10, padding:"9px 18px", cursor:"pointer", whiteSpace:"nowrap", fontSize:13, fontWeight:700, transition:"all .2s", flexShrink:0 }}>
                  {c.label}
                </button>
              ))}
            </div>
          </div>

          {/* Jobs header */}
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", margin:"24px 0 16px" }}>
            <div>
              <div style={{ fontSize:20, fontWeight:800 }}>
                {search||wilaya ? "نتائج البحث" : "أحدث الوظائف"}
                <span style={{ fontSize:13, color:C.gray, fontWeight:600, marginRight:10 }}>({jobs.length} وظيفة)</span>
              </div>
              <div style={{ width:36, height:3, background:C.red, borderRadius:2, marginTop:4 }} />
            </div>
            <button onClick={() => { setCat("all"); setSearch(""); setWilaya(""); }} style={{ color:C.red, background:"none", border:"none", cursor:"pointer", fontSize:13, fontWeight:700 }}>عرض الكل</button>
          </div>

          {/* Jobs Grid */}
          {jobs.length === 0 ? (
            <div style={{ textAlign:"center", padding:"4rem", color:C.gray }}>
              <div style={{ fontSize:"3rem", marginBottom:12 }}>🔍</div>
              <div style={{ fontWeight:700, fontSize:18 }}>لا توجد نتائج</div>
              <div style={{ fontSize:14, marginTop:6 }}>جرب تغيير معايير البحث</div>
            </div>
          ) : (
            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(320px,1fr))", gap:16 }}>
              {jobs.map(j => {
                const isSaved = saved.includes(j.id);
                return (
                  <div key={j.id} className="jcard" style={{ background:"#fff", borderRadius:16, padding:20, border:`1px solid ${C.grayBorder}`, cursor:"pointer", transition:"all .25s", boxShadow:"0 2px 10px rgba(212,23,43,0.05)" }}>
                    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start", marginBottom:14 }}>
                      <div style={{ display:"flex", gap:12, alignItems:"center" }}>
                        <div style={{ width:44, height:44, borderRadius:12, background:`linear-gradient(135deg,${C.redDark},${C.red})`, display:"flex", alignItems:"center", justifyContent:"center", fontSize:16, fontWeight:900, color:"#fff", flexShrink:0 }}>{j.logo}</div>
                        <div>
                          <div style={{ fontSize:12, color:C.gray, fontWeight:600 }}>{j.company}</div>
                          <div style={{ fontSize:15, fontWeight:800, color:C.text }}>{j.title}</div>
                        </div>
                      </div>
                      <div style={{ display:"flex", flexDirection:"column", alignItems:"flex-end", gap:6 }}>
                        {j.urgent && <span style={{ background:`${C.red}15`, color:C.red, fontSize:10, fontWeight:800, padding:"3px 10px", borderRadius:100, border:`1px solid ${C.red}30` }}>⚡ عاجل</span>}
                        <button onClick={e => { e.stopPropagation(); toggleSave(j.id); }} style={{ background:"none", border:"none", cursor:"pointer", color: isSaved ? C.red : C.gray, fontSize:20, padding:2 }}>{isSaved ? "★" : "☆"}</button>
                      </div>
                    </div>

                    <div style={{ display:"flex", gap:6, flexWrap:"wrap", marginBottom:12 }}>
                      {j.tags.map(t => <span key={t} style={{ background:"#FFF0F1", color:C.red, fontSize:11, fontWeight:700, padding:"3px 9px", borderRadius:6 }}>{t}</span>)}
                      {j.remote && <span style={{ background:"#FFF0F1", color:C.red, fontSize:11, fontWeight:800, padding:"3px 9px", borderRadius:100 }}>🌐 عن بُعد</span>}
                    </div>

                    <div style={{ display:"flex", gap:14, fontSize:12, color:C.gray, marginBottom:14, flexWrap:"wrap" }}>
                      <span>📍 {j.location}</span><span>💼 {j.type}</span><span>🕐 {j.posted}</span>
                    </div>

                    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", paddingTop:12, borderTop:`1px solid ${C.grayBorder}` }}>
                      <span style={{ fontSize:13, fontWeight:800, color:C.red }}>{j.salary}</span>
                      <button onClick={() => setJob(j)} style={{ background:`linear-gradient(135deg,${C.redDark},${C.red})`, color:"#fff", border:"none", borderRadius:8, padding:"8px 16px", fontSize:12, fontWeight:800, cursor:"pointer" }}>عرض التفاصيل ←</button>
                    </div>
                  </div>
                );
              })}
            </div>
          )}
        </div>
      )}

      {/* ═══ SAVED ═══ */}
      {tab === "saved" && (
        <div style={{ maxWidth:1200, margin:"0 auto", padding:"28px 20px" }}>
          <div style={{ fontSize:22, fontWeight:800, marginBottom:4 }}>الوظائف المحفوظة</div>
          <div style={{ width:36, height:3, background:C.red, borderRadius:2, marginBottom:20 }} />
          {saved.length === 0 ? (
            <div style={{ textAlign:"center", padding:"4rem", color:C.gray }}>
              <div style={{ fontSize:"3rem", marginBottom:12 }}>☆</div>
              <div style={{ fontWeight:700, fontSize:18, color:C.text }}>لا توجد وظائف محفوظة</div>
              <div style={{ fontSize:14, marginTop:6 }}>احفظ الوظائف التي تهمك</div>
            </div>
          ) : (
            <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(320px,1fr))", gap:16 }}>
              {JOBS.filter(j => saved.includes(j.id)).map(j => {
                const isSaved = true;
                return (
                  <div key={j.id} className="jcard" style={{ background:"#fff", borderRadius:16, padding:20, border:`1px solid ${C.grayBorder}`, cursor:"pointer", transition:"all .25s" }}>
                    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start", marginBottom:14 }}>
                      <div style={{ display:"flex", gap:12, alignItems:"center" }}>
                        <div style={{ width:44, height:44, borderRadius:12, background:`linear-gradient(135deg,${C.redDark},${C.red})`, display:"flex", alignItems:"center", justifyContent:"center", fontSize:16, fontWeight:900, color:"#fff" }}>{j.logo}</div>
                        <div>
                          <div style={{ fontSize:12, color:C.gray, fontWeight:600 }}>{j.company}</div>
                          <div style={{ fontSize:15, fontWeight:800 }}>{j.title}</div>
                        </div>
                      </div>
                      <button onClick={() => toggleSave(j.id)} style={{ background:"none", border:"none", cursor:"pointer", color:C.red, fontSize:20 }}>★</button>
                    </div>
                    <div style={{ display:"flex", gap:14, fontSize:12, color:C.gray, marginBottom:14 }}>
                      <span>📍 {j.location}</span><span>💼 {j.type}</span>
                    </div>
                    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", paddingTop:12, borderTop:`1px solid ${C.grayBorder}` }}>
                      <span style={{ fontSize:13, fontWeight:800, color:C.red }}>{j.salary}</span>
                      <button onClick={() => setJob(j)} style={{ background:`linear-gradient(135deg,${C.redDark},${C.red})`, color:"#fff", border:"none", borderRadius:8, padding:"8px 16px", fontSize:12, fontWeight:800, cursor:"pointer" }}>عرض التفاصيل ←</button>
                    </div>
                  </div>
                );
              })}
            </div>
          )}
        </div>
      )}

      {/* ═══ ABOUT ═══ */}
      {tab === "about" && (
        <div style={{ maxWidth:700, margin:"0 auto", padding:"48px 20px", textAlign:"center" }}>
          <div style={{ width:80, height:80, background:`linear-gradient(135deg,${C.redDark},${C.red})`, borderRadius:20, display:"flex", alignItems:"center", justifyContent:"center", fontSize:32, fontWeight:900, color:"#fff", margin:"0 auto 20px" }}>ت</div>
          <h2 style={{ fontSize:28, fontWeight:900, marginBottom:14 }}>من نحن</h2>
          <p style={{ fontSize:15, lineHeight:2, color:C.textSoft, marginBottom:28 }}>
            <strong style={{ color:C.red }}>توجيه</strong> هي منصة جزائرية متخصص
