import { useState } from "react";

const GOLD = "#C9A84C";
const GOLD2 = "#E8C97A";
const DARK = "#0A0A0A";
const DARK2 = "#141414";
const DARK3 = "#1C1C1C";
const CARD_BG = "#161616";
const BORDER = "#2A2A2A";
const GOLD_BORDER = "#C9A84C44";

const initialProperties = [
  {
    id: 1,
    title: "Azure Sky Residences",
    titleAr: "مجمع أزور سكاي",
    location: "Limassol Marina, Cyprus",
    locationAr: "مرسى ليماسول، قبرص",
    price: "€485,000",
    type: "Apartment",
    bedrooms: 3,
    bathrooms: 2,
    area: 142,
    developer: "Leptos Estates",
    commission: "7%",
    status: "New Launch",
    views: 342,
    whatsapp: "35799000001",
    image: "https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=600&q=80",
    description: "Stunning sea-view apartments in Limassol Marina with world-class amenities.",
    descriptionAr: "شقق فاخرة بإطلالة بحرية في مرسى ليماسول مع مرافق عالمية المستوى.",
    featured: true,
  },
  {
    id: 2,
    title: "Paphos Hills Villa Estate",
    titleAr: "فيلات تلال بافوس",
    location: "Paphos, Cyprus",
    locationAr: "بافوس، قبرص",
    price: "€920,000",
    type: "Villa",
    bedrooms: 5,
    bathrooms: 4,
    area: 380,
    developer: "Aristo Developers",
    commission: "8%",
    status: "Off-Plan",
    views: 218,
    whatsapp: "35799000001",
    image: "https://images.unsplash.com/photo-1512917774080-9991f1c4c750?w=600&q=80",
    description: "Exclusive hilltop villas with panoramic views of the Mediterranean.",
    descriptionAr: "فيلات حصرية على قمة التل مع إطلالات بانورامية على البحر الأبيض المتوسط.",
    featured: false,
  },
  {
    id: 3,
    title: "Nicosia Business Tower",
    titleAr: "برج نيقوسيا التجاري",
    location: "Nicosia City Center",
    locationAr: "وسط مدينة نيقوسيا",
    price: "€310,000",
    type: "Apartment",
    bedrooms: 2,
    bathrooms: 1,
    area: 98,
    developer: "Imperio Properties",
    commission: "6%",
    status: "Ready",
    views: 187,
    whatsapp: "35799000001",
    image: "https://images.unsplash.com/photo-1486325212027-8081e485255e?w=600&q=80",
    description: "Modern city-center apartments ideal for investment and residency.",
    descriptionAr: "شقق عصرية في وسط المدينة مثالية للاستثمار والإقامة.",
    featured: false,
  },
];

export default function App() {
  const [lang, setLang] = useState("en");
  const [view, setView] = useState("listings");
  const [properties, setProperties] = useState(initialProperties);
  const [showForm, setShowForm] = useState(false);
  const [editProp, setEditProp] = useState(null);
  const [shareModal, setShareModal] = useState(null);
  const [viewedProp, setViewedProp] = useState(null);
  const isAr = lang === "ar";

  const [form, setForm] = useState({
    title: "", titleAr: "", location: "", locationAr: "",
    price: "", type: "Apartment", bedrooms: 2, bathrooms: 1,
    area: 100, developer: "", commission: "7%", status: "New Launch",
    whatsapp: "35799000001", image: "", description: "", descriptionAr: "", featured: false,
  });

  const totalViews = properties.reduce((s, p) => s + p.views, 0);
  const topProp = [...properties].sort((a, b) => b.views - a.views)[0];

  const openAdd = () => {
    setForm({ title:"",titleAr:"",location:"",locationAr:"",price:"",type:"Apartment",bedrooms:2,bathrooms:1,area:100,developer:"",commission:"7%",status:"New Launch",whatsapp:"35799000001",image:"",description:"",descriptionAr:"",featured:false });
    setEditProp(null);
    setShowForm(true);
  };
  const openEdit = (p) => { setForm({ ...p }); setEditProp(p.id); setShowForm(true); };
  const saveProp = () => {
    if (editProp) setProperties(ps => ps.map(p => p.id === editProp ? { ...form, id: editProp, views: p.views } : p));
    else setProperties(ps => [...ps, { ...form, id: Date.now(), views: 0 }]);
    setShowForm(false);
  };
  const deleteProp = (id) => setProperties(ps => ps.filter(p => p.id !== id));
  const trackView = (id) => setProperties(ps => ps.map(p => p.id === id ? { ...p, views: p.views + 1 } : p));

  const css = `
    @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600&family=Inter:wght@300;400;500&family=Cairo:wght@400;600&display=swap');
    *{box-sizing:border-box;margin:0;padding:0}
    body{background:${DARK};color:#E8E0D0;font-family:${isAr?"'Cairo','Inter'":"'Inter'"}, sans-serif}
    .gold{color:${GOLD}}
    .btn-gold{background:linear-gradient(135deg,${GOLD},${GOLD2});color:#000;border:none;padding:10px 22px;border-radius:6px;font-weight:500;cursor:pointer;font-size:14px;letter-spacing:0.3px}
    .btn-outline{background:transparent;border:1px solid ${GOLD_BORDER};color:${GOLD};padding:8px 18px;border-radius:6px;cursor:pointer;font-size:13px}
    .btn-ghost{background:transparent;border:1px solid ${BORDER};color:#999;padding:7px 16px;border-radius:6px;cursor:pointer;font-size:13px}
    .card{background:${CARD_BG};border:1px solid ${BORDER};border-radius:12px;overflow:hidden;transition:border 0.2s}
    .card:hover{border-color:${GOLD}55}
    input,select,textarea{background:#1E1E1E;border:1px solid ${BORDER};color:#E8E0D0;padding:9px 13px;border-radius:7px;font-size:13px;width:100%;outline:none;font-family:inherit}
    input:focus,select:focus,textarea:focus{border-color:${GOLD}88}
    label{font-size:12px;color:#888;display:block;margin-bottom:5px;margin-top:14px}
    .badge{display:inline-block;padding:3px 10px;border-radius:20px;font-size:11px;font-weight:500}
    .modal-bg{position:fixed;inset:0;background:#00000099;z-index:100;display:flex;align-items:center;justify-content:center}
    .modal{background:${DARK2};border:1px solid ${BORDER};border-radius:14px;padding:28px;width:95%;max-width:560px;max-height:90vh;overflow-y:auto}
    .nav-link{padding:8px 18px;border-radius:6px;cursor:pointer;font-size:14px;color:#999;border:none;background:transparent}
    .nav-link.active{color:${GOLD};background:#C9A84C15}
    .stat-card{background:${DARK3};border:1px solid ${BORDER};border-radius:10px;padding:18px 22px}
    .grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    .grid3{display:grid;grid-template-columns:repeat(auto-fill,minmax(300px,1fr));gap:20px}
    .share-card{background:#111;border-radius:12px;padding:22px;border:2px solid ${GOLD}44}
    ::-webkit-scrollbar{width:6px} ::-webkit-scrollbar-track{background:#111} ::-webkit-scrollbar-thumb{background:${GOLD}55;border-radius:3px}
  `;

  return (
    <div style={{ minHeight: "100vh", background: DARK, direction: isAr ? "rtl" : "ltr" }}>
      <style>{css}</style>

      <div style={{ background: DARK2, borderBottom: `1px solid ${BORDER}`, padding: "0 24px" }}>
        <div style={{ maxWidth: 1100, margin: "0 auto", display: "flex", alignItems: "center", justifyContent: "space-between", height: 64 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
            <div style={{ width: 36, height: 36, borderRadius: "50%", background: `linear-gradient(135deg,${GOLD},${GOLD2})`, display: "flex", alignItems: "center", justifyContent: "center" }}>
              <span style={{ fontSize: 18 }}>🏛</span>
            </div>
            <div>
              <div style={{ fontFamily: "'Playfair Display', serif", color: GOLD, fontSize: 17, fontWeight: 600 }}>Cyprus Luxury Properties</div>
              <div style={{ fontSize: 11, color: "#666" }}>قبرص للعقارات الفاخرة</div>
            </div>
          </div>
          <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
            <button className="nav-link" style={{ color: lang === "en" ? GOLD : "#666" }} onClick={() => setLang("en")}>EN</button>
            <button className="nav-link" style={{ color: lang === "ar" ? GOLD : "#666" }} onClick={() => setLang("ar")}>AR</button>
            <div style={{ width: 1, height: 20, background: BORDER, margin: "0 8px" }} />
            <button className={`nav-link ${view === "listings" ? "active" : ""}`} onClick={() => setView("listings")}>
              {isAr ? "العقارات" : "Listings"}
            </button>
            <button className={`nav-link ${view === "dashboard" ? "active" : ""}`} onClick={() => setView("dashboard")}>
              {isAr ? "لوحة التحكم" : "Dashboard"}
            </button>
            <button className="btn-gold" onClick={openAdd}>
              + {isAr ? "إضافة عقار" : "Add Property"}
            </button>
          </div>
        </div>
      </div>

      {view === "listings" && (
        <div style={{ background: `linear-gradient(180deg, ${DARK2} 0%, ${DARK} 100%)`, padding: "48px 24px 32px", textAlign: "center" }}>
          <div style={{ fontFamily: "'Playfair Display', serif", fontSize: 36, color: GOLD, marginBottom: 10, letterSpacing: 1 }}>
            {isAr ? "اكتشف أفضل العقارات في قبرص" : "Discover Luxury Properties in Cyprus"}
          </div>
          <div style={{ color: "#888", fontSize: 15, maxWidth: 500, margin: "0 auto" }}>
            {isAr ? "عقارات حصرية مباشرة من كبار المطورين" : "Exclusive developments from top Cyprus developers"}
          </div>
          <div style={{ display: "flex", justifyContent: "center", gap: 32, marginTop: 28 }}>
            {[["🏙", isAr ? "ليماسول" : "Limassol"], ["🌊", isAr ? "بافوس" : "Paphos"], ["🏛", isAr ? "نيقوسيا" : "Nicosia"], ["🌿", isAr ? "لارنكا" : "Larnaca"]].map(([icon, city]) => (
              <div key={city} style={{ textAlign: "center", cursor: "pointer", opacity: 0.8 }}>
                <div style={{ fontSize: 22 }}>{icon}</div>
                <div style={{ fontSize: 12, color: GOLD, marginTop: 4 }}>{city}</div>
              </div>
            ))}
          </div>
        </div>
      )}

      <div style={{ maxWidth: 1100, margin: "0 auto", padding: "24px 24px 60px" }}>
        {view === "dashboard" && (
          <div>
            <div style={{ fontSize: 22, color: GOLD, fontFamily: "'Playfair Display', serif", marginBottom: 24 }}>
              {isAr ? "لوحة التحكم" : "Analytics Dashboard"}
            </div>
            <div className="grid2" style={{ marginBottom: 20 }}>
              <div className="stat-card">
                <div style={{ fontSize: 12, color: "#888", marginBottom: 8 }}>{isAr ? "إجمالي المشاهدات" : "Total Views"}</div>
                <div style={{ fontSize: 36, color: GOLD, fontWeight: 500 }}>{totalViews.toLocaleString()}</div>
              </div>
              <div className="stat-card">
                <div style={{ fontSize: 12, color: "#888", marginBottom: 8 }}>{isAr ? "إجمالي العقارات" : "Active Listings"}</div>
                <div style={{ fontSize: 36, color: GOLD, fontWeight: 500 }}>{properties.length}</div>
              </div>
            </div>
            <div className="stat-card" style={{ marginBottom: 24 }}>
              <div style={{ fontSize: 12, color: "#888", marginBottom: 10 }}>{isAr ? "الأكثر مشاهدة" : "Top Performing Property"}</div>
              <div style={{ fontSize: 16, color: "#E8E0D0", fontWeight: 500 }}>{isAr ? topProp?.titleAr : topProp?.title}</div>
              <div style={{ fontSize: 13, color: GOLD, marginTop: 4 }}>{topProp?.views} {isAr ? "مشاهدة" : "views"}</div>
            </div>
            <div style={{ fontSize: 15, color: "#888", marginBottom: 14 }}>{isAr ? "أداء كل عقار" : "Property Performance"}</div>
            {properties.map(p => (
              <div key={p.id} style={{ background: DARK3, border: `1px solid ${BORDER}`, borderRadius: 10, padding: "14px 18px", marginBottom: 10, display: "flex", alignItems: "center", gap: 16 }}>
                <img src={p.image || "https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=80&q=60"} alt="" style={{ width: 52, height: 52, borderRadius: 8, objectFit: "cover" }} />
                <div style={{ flex: 1 }}>
                  <div style={{ fontSize: 14, color: "#E8E0D0" }}>{isAr ? p.titleAr : p.title}</div>
                  <div style={{ fontSize: 12, color: "#666", marginTop: 2 }}>{isAr ? p.locationAr : p.location}</div>
                </div>
                <div style={{ textAlign: "right" }}>
                  <div style={{ fontSize: 20, color: GOLD, fontWeight: 500 }}>{p.views}</div>
                  <div style={{ fontSize: 11, color: "#555" }}>{isAr ? "مشاهدة" : "views"}</div>
                </div>
                <div style={{ width: 120, height: 8, background: "#222", borderRadius: 4, overflow: "hidden" }}>
                  <div style={{ width: `${Math.min(100, (p.views / Math.max(...properties.map(x => x.views))) * 100)}%`, height: "100%", background: `linear-gradient(90deg,${GOLD},${GOLD2})`, borderRadius: 4 }} />
                </div>
                <div>
                  <div style={{ fontSize: 13, color: "#4CAF50", fontWeight: 500 }}>{p.commission}</div>
                  <div style={{ fontSize: 11, color: "#555" }}>{isAr ? "عمولة" : "commission"}</div>
                </div>
                <button className="btn-ghost" style={{ fontSize: 12 }} onClick={() => openEdit(p)}>✏️</button>
                <button className="btn-ghost" style={{ fontSize: 12, color: "#ff5555", borderColor: "#ff555533" }} onClick={() => deleteProp(p.id)}>🗑</button>
              </div>
            ))}
          </div>
        )}

        {view === "listings" && (
          <div className="grid3">
            {properties.map(p => (
              <PropertyCard key={p.id} p={p} isAr={isAr} onEdit={openEdit} onDelete={deleteProp} onShare={setShareModal} onView={trackView} onOpen={setViewedProp} />
            ))}
          </div>
        )}
      </div>

      {showForm && (
        <div className="modal-bg" onClick={() => setShowForm(false)}>
          <div className="modal" onClick={e => e.stopPropagation()}>
            <div style={{ fontFamily: "'Playfair Display', serif", fontSize: 20, color: GOLD, marginBottom: 20 }}>
              {editProp ? (isAr ? "تعديل العقار" : "Edit Property") : (isAr ? "إضافة عقار جديد" : "Add New Property")}
            </div>
            <div className="grid2">
              <div><label>Title (EN)</label><input value={form.title} onChange={e => setForm(f => ({ ...f, title: e.target.value }))} placeholder="Property name" /></div>
              <div><label>العنوان (AR)</label><input value={form.titleAr} onChange={e => setForm(f => ({ ...f, titleAr: e.target.value }))} placeholder="اسم العقار" dir="rtl" /></div>
              <div><label>Location (EN)</label><input value={form.location} onChange={e => setForm(f => ({ ...f, location: e.target.value }))} /></div>
              <div><label>الموقع (AR)</label><input value={form.locationAr} onChange={e => setForm(f => ({ ...f, locationAr: e.target.value }))} dir="rtl" /></div>
              <div><label>Price</label><input value={form.price} onChange={e => setForm(f => ({ ...f, price: e.target.value }))} placeholder="€000,000" /></div>
              <div><label>Type</label><select value={form.type} onChange={e => setForm(f => ({ ...f, type: e.target.value }))}><option>Apartment</option><option>Villa</option><option>Penthouse</option><option>Commercial</option></select></div>
              <div><label>Bedrooms</label><input type="number" value={form.bedrooms} onChange={e => setForm(f => ({ ...f, bedrooms: +e.target.value }))} /></div>
              <div><label>Bathrooms</label><input type="number" value={form.bathrooms} onChange={e => setForm(f => ({ ...f, bathrooms: +e.target.value }))} /></div>
              <div><label>Area (m²)</label><input type="number" value={form.area} onChange={e => setForm(f => ({ ...f, area: +e.target.value }))} /></div>
              <div><label>Developer</label><input value={form.developer} onChange={e => setForm(f => ({ ...f, developer: e.target.value }))} /></div>
              <div><label>Commission %</label><select value={form.commission} onChange={e => setForm(f => ({ ...f, commission: e.target.value }))}>{["5%","6%","7%","8%","9%","10%"].map(c => <option key={c}>{c}</option>)}</select></div>
              <div><label>Status</label><select value={form.status} onChange={e => setForm(f => ({ ...f, status: e.target.value }))}><option>New Launch</option><option>Off-Plan</option><option>Ready</option></select></div>
            </div>
            <label>WhatsApp Number</label>
            <input value={form.whatsapp} onChange={e => setForm(f => ({ ...f, whatsapp: e.target.value }))} placeholder="35799..." />
            <label>Photo URL</label>
            <input value={form.image} onChange={e => setForm(f => ({ ...f, image: e.target.value }))} placeholder="https://..." />
            <label>Description (EN)</label>
            <textarea rows={2} value={form.description} onChange={e => setForm(f => ({ ...f, description: e.target.value }))} />
            <label>الوصف (AR)</label>
            <textarea rows={2} value={form.descriptionAr} onChange={e => setForm(f => ({ ...f, descriptionAr: e.target.value }))} dir="rtl" />
            <div style={{ display: "flex", gap: 10, marginTop: 22, justifyContent: "flex-end" }}>
              <button className="btn-ghost" onClick={() => setShowForm(false)}>{isAr ? "إلغاء" : "Cancel"}</button>
              <button className="btn-gold" onClick={saveProp}>{isAr ? "حفظ" : "Save Property"}</button>
            </div>
          </div>
        </div>
      )}

      {shareModal && <ShareModal p={shareModal} isAr={isAr} onClose={() => setShareModal(null)} />}
      {viewedProp && <ViewModal p={viewedProp} isAr={isAr} onClose={() => setViewedProp(null)} onShare={setShareModal} />}
    </div>
  );
}

function PropertyCard({ p, isAr, onEdit, onDelete, onShare, onView, onOpen }) {
  return (
    <div className="card" style={{ cursor: "pointer" }} onClick={() => { onView(p.id); onOpen(p); }}>
      <div style={{ position: "relative", height: 200, overflow: "hidden" }}>
        <img src={p.image || "https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=600&q=80"} alt={p.title} style={{ width: "100%", height: "100%", objectFit: "cover" }} />
        <div style={{ position: "absolute", inset: 0, background: "linear-gradient(180deg, transparent 50%, #000000cc 100%)" }} />
        <div style={{ position: "absolute", top: 12, left: 12, display: "flex", gap: 6 }}>
          <span className="badge" style={{ background: `${({"New Launch":GOLD,"Off-Plan":"#4A9EFF","Ready":"#4CAF50"})[p.status]}22`, color: ({"New Launch":GOLD,"Off-Plan":"#4A9EFF","Ready":"#4CAF50"})[p.status], border: `1px solid ${({"New Launch":GOLD,"Off-Plan":"#4A9EFF","Ready":"#4CAF50"})[p.status]}55` }}>{p.status}</span>
          {p.featured && <span className="badge" style={{ background: "#C9A84C22", color: GOLD, border: `1px solid ${GOLD}44` }}>★ {isAr ? "مميز" : "Featured"}</span>}
        </div>
        <div style={{ position: "absolute", top: 12, right: 12, background: "#00000099", borderRadius: 20, padding: "3px 10px", fontSize: 12, color: "#ccc" }}>
          👁 {p.views}
        </div>
        <div style={{ position: "absolute", bottom: 12, left: 12 }}>
          <div style={{ fontSize: 20, color: "#fff", fontWeight: 600, fontFamily: "'Playfair Display',serif" }}>{p.price}</div>
        </div>
      </div>
      <div style={{ padding: "16px" }}>
        <div style={{ fontSize: 15, color: "#E8E0D0", fontWeight: 500, marginBottom: 4 }}>{isAr ? p.titleAr : p.title}</div>
        <div style={{ fontSize: 12, color: "#888", marginBottom: 12 }}>📍 {isAr ? p.locationAr : p.location}</div>
        <div style={{ display: "flex", gap: 14, fontSize: 12, color: "#aaa", marginBottom: 14 }}>
          <span>🛏 {p.bedrooms} {isAr ? "غرفة" : "bed"}</span>
          <span>🚿 {p.bathrooms} {isAr ? "حمام" : "bath"}</span>
          <span>📐 {p.area}m²</span>
        </div>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", borderTop: "1px solid #1E1E1E", paddingTop: 12 }}>
          <div>
            <div style={{ fontSize: 11, color: "#555" }}>{isAr ? "المطور" : "Developer"}</div>
            <div style={{ fontSize: 12, color: GOLD }}>{p.developer}</div>
          </div>
          <div style={{ textAlign: "right" }}>
            <div style={{ fontSize: 11, color: "#555" }}>{isAr ? "عمولتك" : "Your Commission"}</div>
            <div style={{ fontSize: 14, color: "#4CAF50", fontWeight: 600 }}>{p.commission}</div>
          </div>
        </div>
        <div style={{ display: "flex", gap: 8, marginTop: 12 }} onClick={e => e.stopPropagation()}>
          <a href={`https://wa.me/${p.whatsapp}?text=${encodeURIComponent(`I'm interested in ${p.title} - ${p.price}`)}`} target="_blank" rel="noreferrer"
            style={{ flex: 1, background: "#25D36622", color: "#25D366", border: "1px solid #25D36644", borderRadius: 7, padding: "8px 0", textAlign: "center", fontSize: 13, textDecoration: "none", display: "block" }}>
            💬 WhatsApp
          </a>
          <button style={{ flex: 1, background: "#C9A84C15", color: GOLD, border: `1px solid ${GOLD}33`, borderRadius: 7, padding: "8px 0", fontSize: 13, cursor: "pointer" }}
            onClick={e => { e.stopPropagation(); onShare(p); }}>
            📤 {isAr ? "مشاركة" : "Share"}
          </button>
        </div>
        <div style={{ display: "flex", gap: 6, marginTop: 8 }} onClick={e => e.stopPropagation()}>
          <button style={{ flex:1, background:"transparent", border:"1px solid #333", borderRadius:6, padding:"6px 0", fontSize:12, color:"#777", cursor:"pointer" }} onClick={() => onEdit(p)}>✏️ {isAr?"تعديل":"Edit"}</button>
          <button style={{ flex:1, background:"transparent", border:"1px solid #ff444433", borderRadius:6, padding:"6px 0", fontSize:12, color:"#ff5555", cursor:"pointer" }} onClick={() => onDelete(p.id)}>🗑 {isAr?"حذف":"Delete"}</button>
        </div>
      </div>
    </div>
  );
}

function ShareModal({ p, isAr, onClose }) {
  const shareText = `🏛 ${p.title}\n📍 ${p.location}\n💰 ${p.price}\n🛏 ${p.bedrooms} bed | 📐 ${p.area}m²\n✅ ${p.status} | Commission: ${p.commission}\n\nContact via WhatsApp: wa.me/${p.whatsapp}`;
  const igText = `✨ NEW LISTING ✨\n${p.title}\n${p.location}\nFrom ${p.price}\n🔥 ${p.status}\nDM or WhatsApp for details 👇`;
  return (
    <div className="modal-bg" onClick={onClose}>
      <div className="modal" style={{ maxWidth: 480 }} onClick={e => e.stopPropagation()}>
        <div style={{ fontFamily: "'Playfair Display', serif", fontSize: 18, color: GOLD, marginBottom: 18 }}>
          📤 {isAr ? "مشاركة على السوشيال ميديا" : "Share to Social Media"}
        </div>
        <div className="share-card" style={{ marginBottom: 14 }}>
          <div style={{ fontSize: 12, color: "#888", marginBottom: 8 }}>📘 Facebook / WhatsApp</div>
          <pre style={{ fontSize: 13, color: "#E8E0D0", whiteSpace: "pre-wrap", fontFamily: "inherit", lineHeight: 1.7 }}>{shareText}</pre>
          <button className="btn-gold" style={{ marginTop: 12, width: "100%" }} onClick={() => navigator.clipboard.writeText(shareText)}>
            📋 {isAr ? "نسخ النص" : "Copy for Facebook / WhatsApp"}
          </button>
        </div>
        <div className="share-card" style={{ marginBottom: 18 }}>
          <div style={{ fontSize: 12, color: "#888", marginBottom: 8 }}>📸 Instagram Caption</div>
          <pre style={{ fontSize: 13, color: "#E8E0D0", whiteSpace: "pre-wrap", fontFamily: "inherit", lineHeight: 1.7 }}>{igText}</pre>
          <button className="btn-gold" style={{ marginTop: 12, width: "100%" }} onClick={() => navigator.clipboard.writeText(igText)}>
            📋 {isAr ? "نسخ لإنستغرام" : "Copy for Instagram"}
          </button>
        </div>
        <button className="btn-ghost" style={{ width: "100%" }} onClick={onClose}>{isAr ? "إغلاق" : "Close"}</button>
      </div>
    </div>
  );
}

function ViewModal({ p, isAr, onClose, onShare }) {
  return (
    <div className="modal-bg" onClick={onClose}>
      <div className="modal" style={{ maxWidth: 580 }} onClick={e => e.stopPropagation()}>
        <img src={p.image || "https://images.unsplash.com/photo-1545324418-cc1a3fa10c00?w=600"} alt={p.title} style={{ width: "100%", height: 220, objectFit: "cover", borderRadius: 10, marginBottom: 18 }} />
        <div style={{ fontFamily: "'Playfair Display', serif", fontSize: 22, color: GOLD, marginBottom: 6 }}>{isAr ? p.titleAr : p.title}</div>
        <div style={{ fontSize: 13, color: "#888", marginBottom: 14 }}>📍 {isAr ? p.locationAr : p.location}</div>
        <div style={{ fontSize: 26, color: "#E8E0D0", fontWeight: 600, marginBottom: 16 }}>{p.price}</div>
        <div style={{ display: "flex", gap: 20, fontSize: 13, color: "#aaa", marginBottom: 16 }}>
          <span>🛏 {p.bedrooms} {isAr ? "غرف" : "Bedrooms"}</span>
          <span>🚿 {p.bathrooms} {isAr ? "حمامات" : "Bathrooms"}</span>
          <span>📐 {p.area} m²</span>
          <span>🏗 {p.type}</span>
        </div>
        <div style={{ fontSize: 14, color: "#ccc", lineHeight: 1.7, marginBottom: 18 }}>{isAr ? p.descriptionAr : p.description}</div>
        <div style={{ display: "flex", gap: 14, marginBottom: 18 }}>
          <div style={{ background: "#1A1A1A", borderRadius: 8, padding: "10px 16px" }}>
            <div style={{ fontSize: 11, color: "#666" }}>{isAr ? "المطور" : "Developer"}</div>
            <div style={{ fontSize: 14, color: GOLD }}>{p.developer}</div>
          </div>
          <div style={{ background: "#1A1A1A", borderRadius: 8, padding: "10px 16px" }}>
            <div style={{ fontSize: 11, color: "#666" }}>{isAr ? "عمولتك" : "Commission"}</div>
            <div style={{ fontSize: 14, color: "#4CAF50", fontWeight: 600 }}>{p.commission}</div>
          </div>
          <div style={{ background: "#1A1A1A", borderRadius: 8, padding: "10px 16px" }}>
            <div style={{ fontSize: 11, color: "#666" }}>{isAr ? "الحالة" : "Status"}</div>
            <div style={{ fontSize: 14, color: GOLD }}>{p.status}</div>
          </div>
        </div>
        <div style={{ display: "flex", gap: 10 }}>
          <a href={`https://wa.me/${p.whatsapp}?text=${encodeURIComponent(`I'm interested in ${p.title} - ${p.price}`)}`} target="_blank" rel="noreferrer"
            style={{ flex: 1, background: "#25D36622", color: "#25D366", border: "1px solid #25D36655", borderRadius: 8, padding: "12px 0", textAlign: "center", textDecoration: "none", fontWeight: 500 }}>
            💬 WhatsApp
          </a>
          <button style={{ flex: 1, background: "#C9A84C15", color: GOLD, border: `1px solid ${GOLD}44`, borderRadius: 8, padding: "12px 0", cursor: "pointer", fontWeight: 500 }} onClick={() => { onClose(); onShare(p); }}>
            📤 {isAr ? "مشاركة" : "Share"}
          </button>
          <button className="btn-ghost" style={{ padding: "12px 18px" }} onClick={onClose}>✕</button>
        </div>
      </div>
    </div>
  );
}
